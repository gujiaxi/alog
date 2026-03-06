# SKILL_BLOG.md — 博客写作指南

alog博客的写作规范与经验沉淀。

---

## 博客基本信息

- **仓库**：https://github.com/gujiaxi/alog
- **Pages URL**：https://gujiaxi.github.io/alog
- **本地clone**：`/root/.openclaw/workspace/alog-repo`
- **主题**：Minima (classic skin)，Jekyll驱动
- **定位**：个人兴趣博客，涵盖AI技术、游戏、设计思考等

---

## Front Matter规范

```yaml
---
layout: post
title: "文章标题"
date: YYYY-MM-DD
tags: [tag1, tag2]
---
```

- **tags**：每篇2个左右，用英文小写，不要过于细化
  - 技术文章：`AI`, `research`, `programming` 等
  - 游戏文章：`gaming`, `design` 等
  - 不用写中文tag，不用写过于具体的子类型（如`metroidvania`、`pretraining`）
- **title**：中英文均可，可以有副标题（用冒号分隔）
- **date**：文件名日期与front matter日期保持一致
- **不要**在文章中标注"由AI撰写"

---

## 文件命名

格式：`YYYY-MM-DD-slug.md`，slug用英文小写连字符，简洁描述主题。

---

## 图片处理

- **不要**把图片放进仓库
- 优先使用**Steam官方截图CDN**（游戏类文章）：
  ```
  https://shared.akamai.steamstatic.com/store_item_assets/steam/apps/<APP_ID>/ss_<hash>.1920x1080.jpg
  ```
  获取方式：`curl "https://store.steampowered.com/api/appdetails?appids=<APP_ID>&filters=screenshots"`
- 其他来源：Nintendo官方媒体资源、官方press kit图
- 图片格式：`![alt text](URL)` + *斜体图注*

---

## 文章结构

### 通用原则

- **开篇不要废话**：直接切入核心观察或问题，不要用"本文将探讨……"
- **结构清晰**：用`##`二级标题分节，段落不要太长
- **收尾有力**：结尾要有一个落点，可以是个人观点、开放性问题或一句能留下印象的话
- 分隔线`---`可以用来增加节奏感

### 常见文章类型

**观点/评论类**（如游戏评论、设计分析）
- 提出一个有具体内容的核心观点（不是"这游戏很好"，而是"为什么好/好在哪"）
- 用个人第一手体验佐证，比泛泛的介绍更有说服力
- 可以引入对比框架（A vs B，过去 vs 现在）来组织论点
- 结尾回到个人立场，不必强求"客观平衡"

**技术/研究类**（如论文解读）
- 先给出论文的核心贡献，让读者知道"值不值得读这篇博客"
- 重点讲思路和动机，而不是堆砌公式或数字
- 可以加自己的评价：这个方法聪明在哪？局限是什么？

**体验/游记类**
- 用具体细节代替形容词（"险胜最终Boss用了最后一颗子弹"比"战斗很紧张"有力得多）
- 主观感受是最大的价值，不要写成攻略

---

## 语言风格

- **中英文之间不加空格**（isaacgu明确要求）
- 行文简洁，不要堆砌形容词
- 游戏名/专有名词首次出现时可以附英文原名，之后不必重复
- 对话式口吻比学术口吻更适合这个博客

---

## 推送流程

```bash
cd /root/.openclaw/workspace/alog-repo
git add _posts/YYYY-MM-DD-slug.md
git commit -m "Add post: <简短描述>"
git push
```

GitHub Pages会在1-2分钟内自动构建。
