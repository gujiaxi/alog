# SKILL_BLOG.md — 技术博客写作技能沉淀

本文件记录如何为 [gujiaxi/alog](https://github.com/gujiaxi/alog) 撰写高质量技术文章的方法论，**仅涉及文章内容与格式本身**，不涉及网站搭建、主题、样式修改等。

---

## 一、文章结构

### 标准结构模板

```
---
layout: post
title: "标题（简洁、有吸引力）"
date: YYYY-MM-DD
tags: [tag1, tag2]
---

<!--摘要段，1~2句话，交代背景和核心贡献，读者看完就决定是否继续读-->

## 背景

## 方法/核心思路

## 实验结果

## 总结与展望

## 参考文献
```

### 节奏感原则

- **开篇直入**：第一段直接给出"这篇文章要解决什么问题"，不写废话铺垫
- **先结论后推导**：每一节的第一句话就是该节最重要的结论，后面是支撑
- **多用"为什么"**：技术文章容易堆砌"是什么"，要多解释设计决策背后的动机
- **适时给读者定位**：用过渡句告诉读者"接下来我们讲X"，降低阅读负担

---

## 二、图片处理

### 图片来源（优先级排序）

1. **论文原图**：从arXiv HTML页面（`https://arxiv.org/html/xxxx.xxxxxv1`）直接提取图片URL，通常是`https://arxiv.org/html/xxxx.xxxxxv1/extracted_images/figure-N.png`形式
2. **论文项目页**：`https://xxxxx.github.io/` 往往有更精美的效果图和架构图
3. **Hugging Face**：`https://huggingface.co/papers/xxxx.xxxxx` 有时有封面图
4. **自制SVG/Mermaid**：流程图、架构图自己画，避免版权问题

### 图片引用格式

```markdown
![图片描述](图片URL)
*图：描述文字，来源：[论文名](arXiv链接)*
```

**注意**：
- 优先使用 **绝对URL**（指向arXiv/GitHub等稳定源），不要把图片下载到仓库里（增大仓库体积）
- 图片描述（alt text）要有意义，兼顾无障碍访问
- 每张图下面加 `*图：xxx*` 格式的图注（斜体），说明来源

### 找图工作流

```
1. 先开 https://arxiv.org/html/xxxx.xxxxxv1 找论文HTML版
   → 右键图片 → 复制图片地址
2. 找不到HTML版则去项目主页（论文abstract里的"Project Page"链接）
3. 架构图等复杂图，用Mermaid代码块或ASCII art代替
```

### Mermaid流程图示例

````markdown
```mermaid
graph LR
    A[输入图像] --> B[视觉编码器]
    B --> C[Transformer]
    C --> D[解码器]
    D --> E[输出视频]
```
````

---

## 三、公式

博客启用了MathJax 3，支持LaTeX语法。

### 行内公式

```markdown
损失函数为 $\mathcal{L} = \mathcal{L}_{flow} + \lambda \mathcal{L}_{reg}$，其中 $\lambda$ 控制正则化强度。
```

### 独立公式块

```markdown
$$
\mathcal{L}_{flow} = \mathbb{E}_{t,x_0,\epsilon}\left[\left\| v_\theta(x_t, t) - (x_1 - x_0) \right\|^2\right]
$$
```

### 常用符号速查

| 含义 | LaTeX |
|------|-------|
| 期望 | `\mathbb{E}` |
| 正态分布 | `\mathcal{N}` |
| 损失函数 | `\mathcal{L}` |
| 向量/矩阵 | `\mathbf{x}`, `\mathbf{W}` |
| 范数 | `\|x\|_2` |
| 偏导 | `\frac{\partial L}{\partial x}` |
| 属于 | `x \in \mathbb{R}^d` |

---

## 四、表格

适合用来**横向对比多个方法**，是技术文章中最高价值的内容之一。

```markdown
| 方法 | 基础模型 | 控制信号 | 推理速度 | 开源 |
|------|---------|---------|---------|------|
| Hallo2 | SVD | 音频 + 姿态 | ~2s/帧 | ✅ |
| SyncAnyone | Wan 2.1 | 音频 | ~1s/帧 | ❌ |
| LatentSync | LDM | 音频 | ~0.5s/帧 | ✅ |
```

**注意**：
- 表头与内容之间必须有`|---|`分隔行
- 对齐方式：`|:---|`左对齐，`|:---:|`居中，`|---:|`右对齐
- 超过6列的表格考虑拆分，手机端会溢出

---

## 五、代码块

### 带语言标注

````markdown
```python
# Flow Matching训练目标
def flow_matching_loss(model, x0, x1, t):
    xt = (1 - t) * x0 + t * x1      # 线性插值
    vt = x1 - x0                      # 目标速度场
    pred = model(xt, t)
    return F.mse_loss(pred, vt)
```
````

### 支持的常用语言标识

`python`, `bash`, `yaml`, `json`, `javascript`, `cpp`, `markdown`

---

## 六、脚注

用于补充说明、定义术语、标注来源，不打断正文流畅性。

```markdown
这里使用了Flow Matching[^1]替代传统DDPM的扩散过程。

[^1]: Flow Matching是Lipman et al. 2022年提出的生成建模框架，用确定性的ODE轨迹替代随机扩散，推理速度更快、训练更稳定。参见[arXiv:2210.02747](https://arxiv.org/abs/2210.02747)。
```

**注意**：脚注定义可以放在文末，Kramdown会自动渲染到页面底部。

---

## 七、引用格式

### 行内引用

```markdown
RAE（表征自编码器）[^rae] 在图像理解和生成任务上均优于标准VAE。
```

### 参考文献节（文末）

```markdown
## 参考文献

1. Tong et al. (2026). *Beyond Language Modeling: An Exploration of Multimodal Pretraining*. [arXiv:2603.03276](https://arxiv.org/abs/2603.03276)
2. LeCun (2022). *A Path Towards Autonomous Machine Intelligence*. [OpenReview](https://openreview.net/forum?id=BZ5a1r-kVsf)
```

**格式规范**：作者（年份）. *斜体标题*. [来源名称](URL)

---

## 八、写作工作流（以研究论文为例）

```
1. 读论文
   ├─ 先读 Abstract + Introduction + Conclusion（10分钟定性）
   ├─ 重点读 Method 节（核心贡献）
   └─ 扫 Experiments 节（关键数据）

2. 找素材
   ├─ arXiv HTML页提取架构图URL
   ├─ 项目主页找效果图URL
   └─ 整理关键数字（参数量、指标、速度等）

3. 写提纲
   ├─ 一句话核心贡献
   ├─ 3~5个关键点（Why interesting / What's new / How it works / Results / Implications）
   └─ 确定是否需要公式/表格/代码

4. 写正文
   ├─ 先写各节第一句（结论句），再展开
   ├─ 图片和公式穿插在相关段落旁
   └─ 最后写开篇摘要段（写完全文才知道怎么概括）

5. 自检
   ├─ 图片URL是否可访问（curl -I 验证）
   ├─ 公式用 $ 包裹检查语法
   ├─ 表格列数是否一致
   └─ 脚注定义是否都有对应引用
```

---

## 九、文章自检清单

发布前逐项确认：

- [ ] Front matter：`layout`, `title`, `date`, `tags` 都填写了
- [ ] 标题中的特殊字符（冒号/引号）用双引号包住整个title
- [ ] 图片：所有 `![...]()` 的URL能访问，alt text有意义
- [ ] 公式：行内 `$...$`，块级 `$$...$$`，没有多余的空格
- [ ] 表格：每行列数一致，分隔行格式正确
- [ ] 脚注：每个 `[^x]` 都有对应的定义
- [ ] 代码块：有语言标识，缩进正确
- [ ] 参考文献：每个论文有arXiv/DOI链接
- [ ] 中英文之间不加空格
- [ ] 文末有参考文献节

---

## 十、案例参考

已撰写的技术文章：

- **超越语言建模**（arXiv:2603.03276）— 涵盖RAE、MoE、多模态预训练四大发现，含架构图、数据表格、公式

写作时遇到arXiv HTML页面不可用的情况，处理方式：
1. 用`web_fetch`抓Abstract页获取核心信息
2. 用项目主页补充效果图和详细描述
3. 没有图片URL时，用Mermaid/ASCII art自绘架构图
