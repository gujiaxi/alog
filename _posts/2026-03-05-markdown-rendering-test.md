---
layout: post
title: "Markdown渲染测试"
date: 2026-03-05
tags: [markdown, meta, jekyll]
---

本文用于测试博客的Markdown渲染效果，涵盖常用元素。

## 标题层级

# H1 一级标题
## H2 二级标题
### H3 三级标题
#### H4 四级标题

---

## 段落与强调

普通段落文本，支持中英文混排。AI领域的发展日新月异，large language models（LLMs）正在重塑软件开发范式。

**粗体文本**用于强调重要内容，*斜体文本*用于术语或书名，~~删除线~~用于标注过时信息。

---

## 列表

### 无序列表

- Transformer架构
- Diffusion Model
- Autoregressive Generation
  - GPT系列
  - LLaMA系列

### 有序列表

1. 数据准备
2. 预训练
3. 指令微调（SFT）
4. 强化学习对齐（RLHF / DPO）

---

## 代码

行内代码：`attention_mask = torch.ones(batch_size, seq_len)`

代码块（Python）：

```python
import torch
import torch.nn as nn

class SelfAttention(nn.Module):
    def __init__(self, d_model, n_heads):
        super().__init__()
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        self.qkv = nn.Linear(d_model, 3 * d_model)
        self.out = nn.Linear(d_model, d_model)

    def forward(self, x):
        B, T, C = x.shape
        qkv = self.qkv(x).reshape(B, T, 3, self.n_heads, self.d_k)
        q, k, v = qkv.unbind(2)
        scale = self.d_k ** -0.5
        attn = (q @ k.transpose(-2, -1)) * scale
        attn = attn.softmax(dim=-1)
        return (attn @ v).reshape(B, T, C)
```

代码块（Bash）：

```bash
# 安装依赖
pip install torch transformers accelerate

# 启动训练
torchrun --nproc_per_node=8 train.py \
  --model_name_or_path meta-llama/Llama-3-8B \
  --output_dir ./output \
  --num_train_epochs 3
```

---

## 引用

> "Scaling laws suggest that model performance improves predictably with compute, data, and parameters."
>
> — Kaplan et al., *Scaling Laws for Neural Language Models*, 2020

多层引用：

> 第一层引用
>
>> 第二层嵌套引用

---

## 表格

| 模型 | 参数量 | 上下文长度 | 开源 |
|------|--------|-----------|------|
| GPT-4 | ~1.8T（估） | 128K | ✗ |
| LLaMA 3 | 8B / 70B / 405B | 128K | ✓ |
| Qwen 2.5 | 0.5B–72B | 128K | ✓ |
| DeepSeek-V3 | 671B（MoE） | 128K | ✓ |
| Gemma 3 | 1B–27B | 128K | ✓ |

---

## 链接与图片

外部链接：[arXiv cs.AI](https://arxiv.org/list/cs.AI/recent)

图片（来自Wikimedia）：

![Transformer Architecture](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/The-Transformer-model-architecture.png/400px-The-Transformer-model-architecture.png)

---

## 数学公式

Attention计算公式：

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)V
$$

行内公式：模型复杂度为$O(n^2 \cdot d)$，其中$n$为序列长度，$d$为隐层维度。

---

## 任务列表

- [x] 搭建Jekyll博客
- [x] 配置GitHub Pages
- [x] 切换Minima主题
- [x] 修复MathJax渲染
- [x] 新增Archive页面
- [ ] 添加tag索引页
- [ ] 配置自定义域名

---

以上覆盖了博客写作中最常用的Markdown元素，可用于验证主题渲染是否正常。
