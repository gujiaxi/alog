# SKILL_BLOG.md — 博客写作指南

记录为 alog 撰写高质量文章的方法论，**仅涉及文章内容与格式本身**，不涉及网站搭建、主题、样式修改等。

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

**不要**把图片放进仓库，统一使用外部直链。

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

**Switch独占游戏 → Nintendo eShop CDN**（适用于无Steam版的游戏，如风花雪月、宝可梦等）

Step 1：用Nintendo JP搜索API拿图片哈希：
```bash
curl -s "https://search.nintendo.jp/nintendo_soft/search.json?q=<游戏名（日文或英文）>&limit=1&opt_os_type=switch" \
  -H "User-Agent: Mozilla/5.0"
```
返回字段：
- `iurl` → 封面图哈希
- `sslurl` → 局内截图哈希列表（通常6张）

Step 2：拼接CDN地址：
```
https://img-eshop.cdn.nintendo.net/i/{hash}.jpg
```

完整脚本：
```bash
curl -s "https://search.nintendo.jp/nintendo_soft/search.json?q=<游戏名>&limit=1&opt_os_type=switch" \
  -H "User-Agent: Mozilla/5.0" | python3 -c "
import json,sys
d=json.load(sys.stdin)
item=d['result']['items'][0]
base='https://img-eshop.cdn.nintendo.net/i/'
print('封面:', base+item['iurl']+'.jpg')
for h in item.get('sslurl',[]):
    print('截图:', base+h+'.jpg')
"
```

其他来源：Nintendo官方press kit图

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

- **中英文之间不加空格**
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

## 十一、AI写作套路避雷清单

来源：[tropes.fyi](https://tropes.fyi) by ossama.is（[Gist](https://gist.github.com/ossa-ma/f3baa9d25154c33095e22272c631f5a1)）。以下模式**单次使用可能无害，但堆叠或反复使用就是AI味的标志**。写完文章后逐条自查。

### 用词（Word Choice）

- **"quietly"类魔法副词**：用"quietly""deeply""fundamentally""remarkably"给平淡描述注入虚假深意
- **"delve"及其同族**：delve、certainly、utilize、leverage、robust、streamline、harness — AI高频词汇表
- **"tapestry""landscape"**：用华丽名词替代简单词。还有paradigm、synergy、ecosystem、framework
- **"serves as"回避简单动词**：用"serves as""stands as""represents"替代简单的"is"，因为AI的重复惩罚机制推它选更花哨的表达

### 句式（Sentence Structure）

- **否定平行句 "It's not X — it's Y"**：最常见的AI写作标志。伪深刻的重构句式，偶尔一次有效，反复使用是对读者的侮辱。变体包括"not because X, but because Y"和跨句否定
- **"Not X. Not Y. Just Z."**：戏剧性倒计时，否定两件事再揭示真相
- **"The X? A Y."**：自问自答修辞问句，问一个没人在问的问题然后自己回答
- **排比滥用（Anaphora）**：同一句式开头反复出现。"They could... They could... They could..."
- **三段式滥用（Tricolon）**：万物三连。一个三段式优雅，三个三段式背靠背是模式识别失败
- **"It's worth noting"**：空洞过渡词，还有"Importantly""Interestingly""Notably"
- **浅层分析短语**：句末缀一个"-ing"短语假装深入，如"highlighting its importance""reflecting broader trends"
- **虚假范围 "from X to Y"**：X和Y根本不在同一尺度上。"From innovation to cultural transformation" — 中间是什么？什么都没有

### 段落（Paragraph Structure）

- **短句碎片化**：极短句单独成段制造虚假紧迫感。"He published this. Openly. In a book. As a priest." — 没有正常人这样写初稿
- **伪装成散文的列表**：本质是listicle但用"The first... The second... The third..."包装成连续段落

### 语气（Tone）

- **"Here's the kicker"**：虚假悬念过渡，承诺揭秘但交付的是平庸观察。还有"Here's the thing""Here's where it gets interesting"
- **"Think of it as..."**：居高临下的比喻，默认读者需要简化类比才能理解。产出的比喻往往比原概念更难懂
- **"Imagine a world where..."**：AI式未来主义开场白
- **虚假脆弱感**：模拟自省或坦诚，实际毫无风险。"And yes, I'm openly in love with..."
- **"The truth is simple"**：断言某事显而易见而不是去证明它。如果你需要告诉读者你的观点很清楚，它很可能不清楚
- **夸大利害**：一切都是史上最重要的事。API定价的博客写成文明命运的沉思
- **"Let's break this down"**：说教式口吻，假设读者需要手把手引导。还有"Let's unpack this""Let's dive in"
- **模糊归因**：引用不具名的"experts""observers""industry reports"。如果你说不出专家是谁，你就没有来源
- **杜撰概念标签**：在领域词后附加"paradox""trap""creep""divide"，伪装成成熟术语。"the supervision paradox""the acceleration trap"

### 格式（Formatting）

- **破折号上瘾**：正常人一篇用2-3个，AI用20+
- **加粗开头列表**：每个列表项都以粗体短语开头，是AI生成文档/博客/README的典型标志
- **Unicode装饰**：→ 箭头、弯引号等键盘上打不出来的字符。真正写字的人产出的是 -> 和直引号

### 篇章（Composition）

- **分形式总结**：每小节总结一次，每大节再总结一次，全文再总结一次。"What I'm going to tell you; what I'm telling you; what I just told you"
- **死磕一个比喻**：一个比喻用5-10次贯穿全文。正常人引入比喻，用完就走
- **历史类比堆叠**：快速列举历史公司/技术革命建立虚假权威。"Apple didn't build Uber. Facebook didn't build Spotify..."
- **单点稀释**：一个论点换10种说法扩充到4000字。800字能讲清楚的事不需要圆形重复
- **内容重复**：逐字重复整段内容，长文中模型丢失上下文的标志
- **显式收尾**：用"In conclusion""To sum up""In summary"宣告结尾。好文章不需要告诉你它在收尾
- **"Despite its challenges..."**：僵化公式 — 承认问题然后立刻驳回。"Despite these challenges, the initiative continues to thrive."

---

## 十二、发布前自检

- [ ] Front matter：`layout`, `title`, `date`, `tags` 齐全
- [ ] 图片URL均可访问（`curl -I <url>` 验证）
- [ ] 中英文之间无多余空格
- [ ] 表格列数一致，分隔行格式正确
- [ ] 公式语法正确（行内`$`，块级`$$`）
- [ ] 脚注每个`[^x]`都有对应定义

---

## 十三、推送流程

```bash
git add _posts/YYYY-MM-DD-slug.md
git commit -m "Add post: <简短描述>"
git push
```

GitHub Pages会在1-2分钟内自动构建。

---

## 已撰写文章参考

- **超越语言建模**（2026-03-05）— 多模态预训练论文解读，含架构图、数据表格、公式
- **银河与恶魔城：同名之下的两种哲学**（2026-03-06）— 游戏设计评论，含Steam截图，对比分析框架
