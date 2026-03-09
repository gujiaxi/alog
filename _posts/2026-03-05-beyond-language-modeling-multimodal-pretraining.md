---
layout: post
title: "超越语言建模：多模态预训练的系统性探索"
date: 2026-03-05 14:23:07
tags: [AI, research]
---

> **论文**：Beyond Language Modeling: An Exploration of Multimodal Pretraining
> **机构**：FAIR (Meta) & New York University
> **作者**：Shengbang Tong\*, David Fan\*, John Nguyen, Ellis Brown, Gaoyue Zhou 等（含Yann LeCun、Saining Xie）
> **arXiv**：[2603.03276](https://arxiv.org/abs/2603.03276) · 2026年3月3日

---

## 背景：语言模型的天花板

过去几年，大语言模型（LLM）的崛起定义了整个AI领域的发展节奏。然而这项工作开篇便提出一个尖锐的论点：**文本是对现实的有损压缩**。借用柏拉图洞穴之喻，语言模型不过是在掌握墙壁上的影子，而非投射影子的实物本身——它描述现象，却缺失物理世界的几何、因果与高保真动态。

更现实的问题是：高质量文本数据正在接近枯竭[^sutskever2025]。与此同时，视觉世界提供了近乎无限的信号流，直接编码着现实的原始动态。这促使研究者将目光转向**统一多模态预训练**，将视觉信号作为与语言平等的一等公民，而非附属的输入模态。

然而，该领域的设计空间至今仍不透明。现有大多数方法（如BAGEL、Janus等）依赖预训练语言模型初始化，再逐步"适配"为多模态模型。这种方式的问题在于：预训练骨干网络中已经编码的知识会干扰对多模态训练本身的分析，使研究者难以分清哪些能力来自联合训练，哪些仅是语言预训练的遗产。

本文选择从零开始的受控实验，以系统性地揭开这个黑箱。

---

## 框架：Transfusion的统一架构

![论文整体框架：顶部为模型架构，底部为五个研究轴](https://arxiv.org/html/2603.03276v1/x1.png)

*图1：研究总览。顶部为高层模型架构（单自回归模型，文本用next-token预测，视觉用flow matching）；底部为五个研究轴：视觉表示、数据、世界建模、架构、扩展律。*

论文采用**Transfusion框架**[^transfusion]，在单一Decoder-only Transformer中同时处理两种模态：

**语言**：标准自回归next-token预测，最小化交叉熵损失

$$\mathcal{L}_{\text{LM}} = -\sum_{i=1}^{n} \log p_\theta(x_i \mid x_{<i})$$

**视觉**：基于流匹配（Flow Matching）的扩散目标，对图像/视频帧的潜在表示进行预测。设$z_0$为干净潜变量，$\epsilon \sim \mathcal{N}(0, I)$，构造插值$z_t = (1-t)\epsilon + t z_0$，模型学习预测速度场$v_\theta$：

$$\mathcal{L}_{\text{flow}} = \mathbb{E}_{t,z_0,\epsilon}\left[\|v_\theta(z_t, t, \cdot) - (z_0 - \epsilon)\|_2^2\right]$$

联合训练损失为两者的加权组合：

$$\mathcal{L} = \lambda_{\text{LM}}\mathcal{L}_{\text{LM}} + \lambda_{\text{flow}}\mathcal{L}_{\text{flow}}$$

默认设置$\lambda_{\text{LM}}=1.0$，$\lambda_{\text{flow}}=3.0$。

![训练数据示例：文本、视频、图文对、动作条件视频](https://arxiv.org/html/2603.03276v1/x2.png)

*图2：训练数据示例。模型在文本、原始视频、图文对和动作条件导航轨迹上联合训练。*

**架构细节**：默认模型共2.3B参数，每个token激活1.5B参数。关键设计是**模态专用FFN**（modality-specific FFNs）——文本token和视觉token分别使用各自的前馈网络，而非共享参数。

![模态专用FFN vs. 共享FFN对比](https://arxiv.org/html/2603.03276v1/x3.png)

*图3：模态专用FFN一致性地优于共享FFN，同时降低文本困惑度、提升图像生成和VQA性能。*

---

## 发现一：RAE——统一视觉表示的最优解

视觉表示的选择一直是多模态模型设计的核心矛盾：

- **VAE**（如SD-VAE、FLUX.1）：擅长生成，但理解能力弱
- **语义编码器**（如SigLIP 2、DINOv2）：擅长理解，但被认为不适合生成
- 因此，Janus、BAGEL等模型采用**双编码器**架构，代价是大幅增加模型复杂度

本文通过对比实验给出了颠覆性结论：**Representation Autoencoder（RAE）[^rae]以单一编码器同时在理解和生成上超越VAE**。

![RAE vs VAE性能对比](https://arxiv.org/html/2603.03276v1/x4.png)

*图4：RAE（SigLIP 2）在DPGBench、GenEval和VQA上全面领先，同时保持与文本基线相当的困惑度。VAE在生成上并不具备优势。*

RAE的核心思想是：在高维语义潜在空间中运行扩散过程是可行的——流匹配不依赖低维像素空间，同样可以在SigLIP 2的高维特征空间中操作。

| 编码器 | 文本PPL | DPGBench | GenEval | VQA |
|--------|---------|----------|---------|-----|
| SD-VAE | ≈基线 | 低 | 低 | 低 |
| FLUX.1 VAE | ≈基线 | 中 | 中 | 中 |
| DINOv2-L | ≈基线 | 中高 | 中高 | 高 |
| **SigLIP 2 (RAE)** | **≈基线** | **最高** | **最高** | **最高** |

**实践建议**：使用单一RAE编码器（如SigLIP 2）替代双编码器架构，可简化模型设计同时提升性能。

---

## 发现二：视觉与语言是互补的，不是竞争的

长期以来，研究者担忧"模态税"（modality tax）——加入视觉数据是否必然损害语言性能？本文给出了细致的实证回答：**不会，但要区分数据类型。**

![视觉数据不与文本竞争](https://arxiv.org/html/2603.03276v1/x5.png)

*图5：Text+Video在DCLM文本困惑度上与文本基线持平。"模态税"的真正来源是图文对中的文本分布偏移，而非视觉信号本身。*

- 加入原始视频数据（Text + Video）可以**匹配甚至微超**文本基线的困惑度
- "模态税"的真正来源是**图文对中的文本分布偏移**（图像描述文本与网络文本分布不同），而非视觉信号本身

更重要的是，实验发现了**跨模态协同效应**：

![多模态协同：通用预训练优于专项扩展](https://arxiv.org/html/2603.03276v1/x9.png)

*图9：通用预训练优于专项扩展。用20B VQA数据 + 通用预训练数据（文本/视频/图文），优于将VQA数据单独扩展至100B。*

1. 图像生成质量随加入文本token的增加而提升（视觉受益于语言）
2. 在VQA任务上，用20B VQA数据 + 通用预训练数据，**优于**单纯将VQA数据扩展到100B

---

## 发现三：世界模型能力从通用预训练中涌现

论文在**导航世界模型（NWM）**[^nwm]设置下测试了一个有趣的假设：统一多模态模型能否在不修改架构的情况下学会世界建模？

NWM任务：给定若干上下文帧 + 导航动作（以文本token表示），预测下一个视觉状态。动作可以是WASD键盘指令，也可以是任意自然语言描述。

![动作以文本token编码](https://arxiv.org/html/2603.03276v1/x11.png)

*图11：动作直接编码为文本token，格式为 I+T→I，无需专门的动作编码器。*

![非监督视频数据驱动世界建模](https://arxiv.org/html/2603.03276v1/x12.png)

*图12：非监督视频数据（蓝色）对世界建模性能的贡献远超其他数据类型，显著优于仅扩展领域内NWM数据。*

![世界建模以极少领域数据即可迁移](https://arxiv.org/html/2603.03276v1/x13.png)

*图13：在固定总训练量的情况下，仅需1%的领域内数据，性能便趋于饱和。核心能力来自通用多模态预训练。*

![自然语言控制的零样本泛化](https://arxiv.org/html/2603.03276v1/x14.png)

*图14：给定4帧上下文，模型能够依据自由文本和WASD动作预测未来帧，支持反事实轨迹生成。*

---

## 发现四：MoE自然诱导模态专业化

模态专用FFN是一个好的起点，但本质上是**手工设计**的二分法，且将容量平均分配给两种模态。**混合专家（MoE）**提供了一个更优雅的替代方案：通过数据驱动的路由，让模型自己学会如何分配专家。

![MoE稀疏扩展持续提升多模态性能](https://arxiv.org/html/2603.03276v1/x16.png)

*图16：固定激活计算量（16个激活专家），将总专家数从32扩展至1008，语言（PPL↓）和视觉（扩散loss↓、GenEval↑）性能持续提升。*

![模态专业化自然涌现](https://arxiv.org/html/2603.03276v1/x18.png)

*图18：专家专业化自然形成。大多数专家聚焦文本，但视觉和多模态专家的比例随网络深度增加而提升，无需任何显式约束。*

**核心结论：**

- 无需显式约束，MoE模型自发形成模态专业化分工
- 视觉理解专家与视觉生成专家高度重合（跨层相关系数$r > 0.9$）
- 在固定激活参数下，将总专家数从32扩展至1008，性能持续提升，训练/推理成本不变
- RAE编码器能够充分利用MoE扩展（扩散loss持续下降），而VAE编码器在高专家数时停滞

---

## 发现五：视觉的扩展律与模态不对称性

论文通过**IsoFLOP分析**（固定计算量，改变模型规模与数据量的分配）推导了统一多模态预训练中视觉与语言各自的扩展律，揭示了一个重要的不对称性：

> **视觉比语言更"数据饥渴"（data-hungry）。**

语言模型在相对适量的数据下便能充分利用模型容量；视觉模型则需要**显著更多的数据**才能让同等规模的模型达到饱和。

这一不对称性带来了架构设计挑战：如果用一个密集模型统一处理两种模态，要么为了语言过配视觉容量，要么为了视觉欠配语言容量。**MoE的解决方案正在于此**：语言token路由到少数高容量专家，视觉token激活更多专家消化海量数据，两种模态在同一激活计算量下各取所需。

---

## 整体意义与展望

本文的贡献不是提出一个新的SOTA模型，而是通过严格受控的实验为统一多模态预训练提供了**系统性的科学理解**。四个核心发现可以提炼为四条设计原则：

| # | 原则 | 核心结论 |
|---|------|----------|
| 1 | **视觉表示** | 用RAE（如SigLIP 2）替代VAE，单编码器同时支持理解和生成 |
| 2 | **数据策略** | 视觉与语言互补，大胆使用原始视频，图文对注意文本分布 |
| 3 | **世界建模** | 通用多模态预训练即可涌现世界模型，无需领域专用架构 |
| 4 | **架构扩展** | MoE比密集模型更适合多模态，自然解决模态不对称的扩展问题 |

从更宏观的视角看，这项工作表明"超越语言建模"的路径已经清晰：视觉数据不是语言数据的噪声，而是互补的信号源；统一多模态预训练不是在语言能力上妥协，而是能实现真正的协同增益。未来的挑战在于如何将这些从零预训练的实验结论扩展到工业级规模——届时，文中揭示的扩展律和架构原则将成为宝贵的指引。

---

## 参考文献

[^sutskever2025]: Sutskever, I. (2025). *Reflections on the limits of text data scaling*. Referenced in arXiv:2603.03276.

[^transfusion]: Zhou, C. et al. (2025). *Transfusion: Predict the Next Token and Diffuse Images with One Multi-Modal Model*. arXiv:2408.11039.

[^rae]: Zheng, B. et al. (2026); Tong, S. et al. (2026). *Representation Autoencoder (RAE)*. Referenced in arXiv:2603.03276.

[^nwm]: Bar, A. et al. (2025). *Navigation World Models*. arXiv:2403.12944.

[^genie]: Bruce, J. et al. (2024). *Genie: Generative Interactive Environments*. arXiv:2402.15391.

[^vaswani2017]: Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017. arXiv:1706.03762.

[^siglip2]: Tschannen, M. et al. (2025). *SigLIP 2: Multilingual Vision-Language Encoders with Improved Semantic Understanding, Localization, and Dense Features*. arXiv:2502.14786.

[^flowmatching]: Lipman, Y. et al. (2023). *Flow Matching for Generative Modeling*. ICLR 2023. arXiv:2210.02747.
