# SKILL_BLOG.md — 博客写作指南

记录如何为 [gujiaxi/alog](https://github.com/gujiaxi/alog) 撰写高质量文章的方法论，**仅涉及文章内容与格式本身**，不涉及网站搭建、主题、样式修改等。

---

## 博客基本信息

- **仓库**：https://github.com/gujiaxi/alog
- **Pages URL**：https://gujiaxi.github.io/alog
- **本地clone**：`/root/.openclaw/workspace/alog-repo`
- **主题**：Minima (classic skin)，Jekyll驱动
- **定位**：个人兴趣博客，涵盖AI技术、游戏、设计思考等

---

## 一、Front Matter规范

```yaml
---
layout: post
title: "标题（简洁、有吸引力）"
date: YYYY-MM-DD
tags: [tag1, tag2]
---
```

- **tags**：每篇2个左右，用英文小写，不要过于细化
  - 技术文章：`AI`, `research`, `programming` 等
  - 游戏文章：`gaming`, `design` 等
  - 不用中文tag，不用写过于具体的子类型（如`metroidvania`、`pretraining`）
- **title**：中英文均可，含冒号/引号时用双引号包住整个title
- **date**：文件名日期与front matter日期保持一致
- **不要**在文章中标注"由AI撰写"

---

## 二、文件命名

格式：`YYYY-MM-DD-slug.md`，slug用英文小写连字符，简洁描述主题。

---

## 三、文章结构

### 通用原则

- **开篇不要废话**：第一段直接切入核心观察或问题，不写"本文将探讨……"
- **先结论后推导**：每一节的第一句话就是该节最重要的结论，后面是支撑
- **多用"为什么"**：技术文章容易堆砌"是什么"，要多解释设计决策背后的动机
- **收尾有力**：结尾要有一个落点，可以是个人观点、开放性问题或一句能留下印象的话
- 分隔线`---`可以用来增加节奏感

### 常见文章类型

**观点/评论类**（如游戏评论、设计分析）
- 提出一个有具体内容的核心观点（不是"这游戏很好"，而是"为什么好/好在哪"）
- 用个人第一手体验佐证，比泛泛介绍更有说服力
- 可以引入对比框架（A vs B，过去 vs 现在）来组织论点
- 结尾回到个人立场，不必强求"客观平衡"

**技术/研究类**（如论文解读）
- 先给出核心贡献，让读者知道"值不值得读这篇博客"
- 重点讲思路和动机，而不是堆砌公式或数字
- 标准结构：摘要段 → 背景 → 方法/核心思路 → 实验结果 → 总结与展望 → 参考文献
- 可以加自己的评价：这个方法聪明在哪？局限是什么？

**体验/游记类**
- 用具体细节代替形容词（"险胜最终Boss用了最后一颗子弹"比"战斗很紧张"有力得多）
- 主观感受是最大的价值，不要写成攻略

---

## 四、图片处理

**不要**把图片放进仓库。

### 图片来源（按文章类型）

**游戏类文章 → Steam官方截图CDN**

```
https://shared.akamai.steamstatic.com/store_item_assets/steam/apps/<APP_ID>/ss_<hash>.1920x1080.jpg
```

获取方式：
```bash
curl "https://store.steampowered.com/api/appdetails?appids=<APP_ID>&filters=screenshots" | python3 -c "
import json,sys
d=json.load(sys.stdin)
for s in list(d.values())[0]['data']['screenshots'][:3]:
    print(s['path_full'])
"
```

其他来源：Nintendo官方媒体资源、官方press kit图

**技术/研究类文章 → arXiv HTML页或项目主页**（优先级排序）

1. arXiv HTML页（`https://arxiv.org/html/xxxx.xxxxxv1`）直接提取图片URL
2. 论文项目主页（Abstract页的"Project Page"链接）
3. Hugging Face（`https://huggingface.co/papers/xxxx.xxxxx`）封面图
4. 自制Mermaid/ASCII art：流程图、架构图

### 图片引用格式

```markdown
![图片描述](图片URL)
*图：描述文字（技术文章可注明来源）*
```

---

## 五、公式（技术文章）

博客启用了MathJax 3，支持LaTeX。

- 行内：`$\mathcal{L} = \mathcal{L}_{flow} + \lambda \mathcal{L}_{reg}$`
- 块级：`$$\mathcal{L}_{flow} = \mathbb{E}[...]$$`

常用符号：`\mathbb{E}` `\mathcal{N}` `\mathcal{L}` `\mathbf{x}` `\frac{\partial L}{\partial x}`

---

## 六、表格

适合横向对比多个方法，是技术文章中最高价值的内容之一。超过6列考虑拆分（手机端会溢出）。

```markdown
| 方法 | 基础模型 | 控制信号 | 开源 |
|------|---------|---------|------|
| MethodA | GPT-4 | 音频 | ✅ |
| MethodB | LLaMA | 视觉 | ❌ |
```

---

## 七、代码块

````markdown
```python
# 带语言标识
def example():
    pass
```
````

常用语言标识：`python`, `bash`, `yaml`, `json`, `javascript`, `cpp`

---

## 八、脚注与引用

```markdown
这里使用了Flow Matching[^1]替代传统DDPM。

[^1]: Flow Matching是Lipman et al. 2022年提出的框架。参见[arXiv:2210.02747](https://arxiv.org/abs/2210.02747)。
```

参考文献节（文末）格式：
```markdown
## 参考文献

1. 作者 (年份). *斜体标题*. [来源](URL)
```

---

## 九、语言风格

- **中英文之间不加空格**（isaacgu明确要求）
- 行文简洁，不堆砌形容词
- 游戏名/专有名词首次出现时可附英文原名，之后不必重复
- 对话式口吻比学术口吻更适合这个博客

---

## 十、写作工作流

**研究论文类**
```
1. 读论文：Abstract + Introduction + Conclusion（定性）→ Method（核心）→ Experiments（数据）
2. 找素材：arXiv HTML页取架构图URL → 项目主页补效果图 → 整理关键数字
3. 写提纲：一句话核心贡献 + 3~5个关键点
4. 写正文：先写各节第一句（结论句），再展开；图片和公式穿插在相关段落旁
5. 最后写开篇摘要段（写完全文才知道怎么概括）
```

**游戏/评论类**
```
1. 确定核心观点（一句话能说清楚的角度）
2. 列出支撑论点的具体游戏/体验作为例证
3. 考虑是否有对比框架可以用（两种设计哲学、古今变化等）
4. 找配图（Steam API获取截图URL）
5. 写作，结尾回到个人立场
```

---

## 十一、发布前自检

- [ ] Front matter：`layout`, `title`, `date`, `tags` 齐全
- [ ] 图片URL均可访问（`curl -I <url>` 验证）
- [ ] 中英文之间无多余空格
- [ ] 表格列数一致，分隔行格式正确
- [ ] 公式语法正确（行内`$`，块级`$$`）
- [ ] 脚注每个`[^x]`都有对应定义

---

## 十二、推送流程

```bash
cd /root/.openclaw/workspace/alog-repo
git add _posts/YYYY-MM-DD-slug.md
git commit -m "Add post: <简短描述>"
git push
```

GitHub Pages会在1-2分钟内自动构建。

---

## 已撰写文章参考

- **超越语言建模**（2026-03-05）— 多模态预训练论文解读，含架构图、数据表格、公式
- **银河与恶魔城：同名之下的两种哲学**（2026-03-06）— 游戏设计评论，含Steam截图，对比分析框架
