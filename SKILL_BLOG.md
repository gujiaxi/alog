# SKILL_BLOG.md — 博客写作指南

为alog撰写文章的方法论，只管文章内容与格式本身；网站搭建、主题和样式不在这里讨论。

## Front Matter

```yaml
---
layout: post
title: "标题"
date: YYYY-MM-DD
tags: [tag1, tag2]
---
```

- tags：每篇2个左右，英文小写，粒度放粗。按大类打，比如`AI`、`research`、`programming`、`gaming`、`design`、`essay`，不用中文tag，也不细化到子类型。
- title：中英文均可；含冒号或引号时用双引号包住整个title。
- date：与文件名中的日期一致。
- 文章中不标注“由AI撰写”。

## 文件命名

`_posts/YYYY-MM-DD-slug.md`，slug用英文小写加连字符，简短描述主题。

## 文章结构

通用原则：

- 开篇直接切入核心观察或问题，不写“本文将探讨……”。
- 每一节的第一句话就是该节最重要的结论，后面再给支撑。
- 多解释“为什么”。技术类文章容易堆“是什么”，设计决策背后的动机更值得写。
- 结尾要有落点：一个观点、一个开放问题，或一句能留下印象的话。不必单独写总结段。

标题与分节：

- 章节标题用`##`，需要再分层时用`###`，一般不超过两级。
- 标题只写内容本身，不加“一、二、三”“1.”“1.1”之类的序号。
- 节与节之间靠标题分隔，不额外插入`---`。分隔线只在正文确实需要场景切换、且那里没有标题时才用，一篇文章里最多一两处。

常见文章类型：

- 观点与评论：提出一个有具体内容的核心观点，讲清楚好在哪、为什么好。用第一手体验做证据，可以借助对比框架（两种做法、过去与现在）组织论点，结尾回到个人立场，不强求“客观平衡”。
- 技术与研究解读：先交代核心贡献，让读者判断值不值得往下读；重点讲思路和动机，少堆公式和数字。常见顺序是背景、核心方法、关键结果、评价与局限、参考文献。自己的评价最有价值：方法聪明在哪，短板在哪。
- 体验与随笔：用具体细节代替形容词，主观感受是最大的价值，不写成攻略或流水账。

## 图片

不把图片放进仓库，统一使用外部直链，发布前确认链接可访问。

来源按文章类型选：

- 游戏：Steam商店截图，可用官方API拿到原图地址：

  ```bash
  curl -s "https://store.steampowered.com/api/appdetails?appids=<APP_ID>&filters=screenshots" | python3 -c "
  import json,sys
  d=json.load(sys.stdin)
  for s in list(d.values())[0]['data']['screenshots'][:3]:
      print(s['path_full'])
  "
  ```

  没有Steam版的主机游戏，可以用任天堂日本官网的搜索接口取eShop图片哈希，再拼成CDN地址：

  ```bash
  curl -s "https://search.nintendo.jp/nintendo_soft/search.json?q=<游戏名>&limit=1&opt_os_type=switch" \
    -H "User-Agent: Mozilla/5.0" | python3 -c "
  import json,sys
  item=json.load(sys.stdin)['result']['items'][0]
  base='https://img-eshop.cdn.nintendo.net/i/'
  print('封面:', base+item['iurl']+'.jpg')
  for h in item.get('sslurl',[]):
      print('截图:', base+h+'.jpg')
  "
  ```

  也可以用厂商官方的press kit。
- 论文与技术：依次尝试arXiv HTML版（`https://arxiv.org/html/<id>`）中的图、论文项目主页、Hugging Face Papers页面的封面图。

引用格式：

```markdown
![图片描述](图片URL)
*图：说明文字（可注明来源）*
```

## 公式

博客启用了MathJax 3。行内用`$...$`，块级用`$$...$$`。

## 表格

适合横向对比多个对象。超过6列就考虑拆分，否则手机上会溢出。

```markdown
| 名称 | 维度A | 维度B |
|------|------|------|
| X    | ...  | ...  |
```

## 代码块

代码块标注语言，如`python`、`bash`、`yaml`、`json`、`javascript`。

## 脚注与参考文献

```markdown
正文中的引用[^1]。

[^1]: 说明文字，附上[来源链接](URL)。
```

文末参考文献：

```markdown
## 参考文献

1. 作者 (年份). *标题*. [来源](URL)
```

## 语言风格

- 中英文之间不加空格。
- 行文简洁，少用形容词。
- 专有名词首次出现时可附原名，之后不必重复。
- 用对话式口吻，不用学术腔。

## 写作流程

研究与技术类：

1. 先读摘要、引言和结论建立判断，再读方法部分抓核心，最后看实验里的关键数据。
2. 收集素材：图片链接、关键数字、需要引用的原文。
3. 写提纲：一句话核心贡献，加3到5个要点。
4. 先写每一节的第一句（结论句），再展开；图片和公式放在相关段落旁边。
5. 最后写开篇段落，写完全文才知道怎么概括。

评论与随笔类：

1. 确定一句话就能说清楚的核心观点。
2. 列出支撑观点的具体经历或作品。
3. 判断有没有合适的对比框架。
4. 准备配图。
5. 写作，结尾回到个人立场。

## AI写作套路避雷清单

来源：[tropes.fyi](https://tropes.fyi) by ossama.is（[Gist](https://gist.github.com/ossa-ma/f3baa9d25154c33095e22272c631f5a1)）。以下模式单次使用可能无害，堆叠或反复出现就是AI味的标志。写完后逐条自查。

### 用词

- “quietly”类魔法副词：用“quietly”“deeply”“fundamentally”“remarkably”给平淡描述注入虚假深意。
- “delve”及其同族：delve、certainly、utilize、leverage、robust、streamline、harness，都是AI高频词。
- “tapestry”“landscape”：用华丽名词替代简单词，同类还有paradigm、synergy、ecosystem、framework。
- “serves as”回避简单动词：用“serves as”“stands as”“represents”代替简单的“is”。

### 句式

- 否定平行句“It's not X — it's Y”：最常见的AI写作标志，伪深刻的重构句式。变体包括“not because X, but because Y”和跨句否定。
- “Not X. Not Y. Just Z.”：戏剧性倒计时，先否定两件事再揭示真相。
- “The X? A Y.”：自问自答，问一个没人在问的问题再自己回答。
- 排比滥用：同一句式开头反复出现。
- 三段式滥用：万物三连。一个三段式优雅，连续几个就是模式化。
- “It's worth noting”：空洞过渡词，同类还有“Importantly”“Interestingly”“Notably”。
- 浅层分析短语：句末缀一个“-ing”短语假装深入，如“highlighting its importance”。
- 虚假范围“from X to Y”：X和Y根本不在同一尺度上。

### 段落

- 短句碎片化：极短句单独成段，制造虚假紧迫感。
- 伪装成散文的列表：本质是清单，用“第一……第二……第三……”包装成段落。

### 语气

- “Here's the kicker”：虚假悬念过渡，同类还有“Here's the thing”。
- “Think of it as...”：居高临下的比喻，常常比原概念更难懂。
- “Imagine a world where...”：AI式未来主义开场。
- 虚假脆弱感：模拟坦诚，实际毫无风险。
- “The truth is simple”：断言显而易见，而不去证明。
- 夸大利害：把小事写成关乎文明命运。
- “Let's break this down”：说教式引导，同类还有“Let's unpack this”“Let's dive in”。
- 模糊归因：引用不具名的“专家”“观察者”“行业报告”。说不出是谁，就等于没有来源。
- 杜撰概念标签：在领域词后加“paradox”“trap”“divide”，伪装成成熟术语。

### 格式

- 破折号上瘾：正常人一篇用两三个，AI能用二十多个。
- 加粗开头列表：每个列表项都以粗体短语开头。
- 用分隔线和序号标题撑结构：每节之间都插`---`，标题都挂“一、二、三”。

### 篇章

- 分形式总结：每小节、每大节、全文各总结一遍。
- 死磕一个比喻：同一个比喻贯穿全文反复用。
- 历史类比堆叠：快速罗列历史公司或技术革命，建立虚假权威。
- 单点稀释：一个论点换十种说法扩写。
- 内容重复：整段重复。
- 显式收尾：用“总之”“综上所述”宣告结尾。
- “Despite its challenges...”：承认问题后立刻驳回的公式化句子。

## 发布前自检

- [ ] Front matter中`layout`、`title`、`date`、`tags`齐全，日期与文件名一致
- [ ] 图片链接都能访问（`curl -I <url>`）
- [ ] 中英文之间没有多余空格
- [ ] 标题没有序号，节与节之间没有多余的`---`
- [ ] 表格列数一致，分隔行格式正确
- [ ] 公式语法正确
- [ ] 每个`[^x]`都有对应的脚注定义

## 发布

```bash
git add _posts/YYYY-MM-DD-slug.md
git commit -m "Add post: <简短描述>"
git push
```

推送到`main`后，GitHub Actions会构建并部署到GitHub Pages，通常几分钟内生效。
