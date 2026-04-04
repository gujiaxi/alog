---
layout: post
title: "摩擦、困难与思考：AI时代的认知自律"
date: 2026-04-04
tags: [AI, essay]
---

瑞典正在把课本发回教室。2022年，前教育部长Lotta Edholm公开称瑞典学校的数字化是一场"没有科学依据的实验"[^1]。政府随后拨款数亿克朗购买纸质教材，禁止课堂使用手机，计划2026年在全国学校实施全天手机禁令。这个故事被广泛引用为"低效打败高效"的证据：纸笔虽然慢，但恰恰是这种慢让学生有时间消化信息。

这个叙事很诱人。它迎合了一个直觉上说得通的框架——Robert Bjork在1994年提出的"desirable difficulty"（合意困难）[^2]：某些看似阻碍学习的困难，实际上能增强长期记忆和知识迁移。瑞典的案例似乎就是活生生的例证。再加上微软2025年的一项研究发现AI正在削弱知识工作者的批判性思维[^3]，以及"不要让AI替你写作"这类呼声[^4]，一条论证链浮出水面：科技消除了必要的摩擦（friction），人脑因此退化，我们需要重新拥抱低效。

但如果这条链条的每一环都比表面看起来更脆弱呢？

---

## 瑞典的故事远比叙事复杂

先看数据。瑞典PISA成绩从2000年到2012年持续下滑，2012至2018年有所回升，2022年再次下降——回到了2012年的低谷。但2022年的下降是全OECD范围内的现象，几乎所有参与国的成绩都在下滑，疫情是公认的主要因素之一[^5]。瑞典在这轮下降中并不特殊。

更值得注意的是，马尔默大学教育学教授Anders Jakobsson在分析PISA 2022结果时指出：瑞典教育问题的核心是**学校间的社会经济隔离不断加剧**——弱势背景学生集中在同一批学校，优质教师分布不均。他说："自2009年以来我一直在说同一件事，如果不解决日益严重的学校隔离问题，其他一切都无法奏效。"[^6]

OECD对瑞典的教育诊断报告更详细地拆解了这些问题：1990年代的去中心化改革、择校制度引入的市场竞争、私立学校（friskolor）的分数膨胀、合格教师仅占70%的师资危机——这些结构性问题都远早于、也远深于"给了学生iPad"这一个变量[^5]。

把瑞典的成绩下降归因于"数字化教育"，就像把一个多年饮食失衡、缺乏运动的人的体检异常归因于"最近换了一双新鞋"。鞋可能确实不合脚，但它不是主要问题。

瑞典政府回收课本的决定更像一次政治纠偏——纠正一个**确实没有循证基础就全面推开**的数字化政策——而不是一个受控实验的结论。这两件事的区别很重要。

---

## "手写优于打字"没那么板上钉钉

Mueller和Oppenheimer在2014年发表了一篇影响深远的研究，标题就叫"笔比键盘更强"[^7]。核心发现是：用笔记本电脑打字的学生在概念性问题上表现更差，因为打字速度快导致了逐字抄录（verbatim transcription），而手写的慢迫使学生做概括和重组——这种深层加工有利于学习。

这个结论广为传播。但2019年，Morehead、Dunlosky和Rawson使用相同的材料和方法做了直接复制实验，**未能重现原始发现**[^8]。他们发现：手写组和打字组在概念性问题上没有显著差异；在延迟两天后的测试中，差异进一步缩小；当允许复习笔记时，两种方法几乎完全等效。他们的结论很直白："基于现有证据，断定哪种方法更优还为时过早。"

更有意思的是，即使在Mueller和Oppenheimer的原始研究中，当他们明确告知打字组"不要逐字记录"时，这个干预完全无效——打字组的逐字率没有任何下降[^7]。这暗示问题可能不在介质本身，而在使用策略。手写之所以（可能）有优势，不是因为"笔"这个物理工具有什么魔力，而是因为手写的物理限制恰好迫使了一种特定的认知行为。如果你能在打字时主动做同样的加工，效果未必不同。

---

## Desirable difficulty的限定条件

Bjork的desirable difficulty框架经常被简化引用，但他本人的表述比流行版本谨慎得多。他在多篇文献中反复强调一个关键限定：

> "The word *desirable* is important. Many difficulties are undesirable during instruction and forever after. If the learner does not have the background knowledge or skills to respond to them successfully, they become undesirable difficulties."[^2]

困难只在学习者**有能力回应**时才是"合意"的。让一个从未游过泳的人跳进深水区不是desirable difficulty，那只是difficulty。这个限定条件把很多草率的推广都挡在了门外。

Bjork具体推荐的四种策略是：间隔练习（spacing）、交错练习（interleaving）、变换学习情境和检索练习（用测试代替重读）。这些都有扎实的实验证据，但它们的共同机制不是"让事情变慢"——而是**迫使更深层的编码和提取加工**[^9]。

换句话说，desirable difficulty的核心不是difficulty本身，而是difficulty所触发的cognitive engagement。这两件事经常重合，但并不等价。

---

## 微软研究揭示的真正变量

Lee等人2025年在CHI发表的研究或许是目前最有分量的AI与认知关系的实证工作[^3]。他们调查了319名知识工作者在936个真实AI使用场景中的批判性思维表现，发现了一个信心-能力交互效应：

- **对AI的信心越高 → 批判性思维越少**
- **对自身能力的信心越高 → 批判性思维越多**

这不是在说"AI让人变笨"。它在说：AI改变了人脑的成本-收益计算。当你面前有一个看起来不错的答案时，继续思考的边际收益（在你看来）下降了。对于本就信任AI胜过信任自己的人，这种效应被放大；对于对自身专业有信心的人，AI反而可能激发更严格的审视——因为他们有能力判断AI的输出，也有动力这样做。

研究还发现，AI把知识工作的认知模式从"执行任务"（task execution）转变为"监督任务"（task stewardship）——从自己做变成审查AI做的结果[^3]。这种转变本身不是问题。问题在于，如果你在一个领域还没有足够的积累来做有效的监督，那你实际上是在监督你不理解的东西。

---

## 那么摩擦到底有什么用？

综合以上证据，我的判断是：

**摩擦本身没有价值。有价值的是摩擦所迫使的认知参与。**

手写笔记之所以（可能）优于打字，不是因为"慢"本身好，而是因为慢**迫使**你做概括、做取舍、做重新组织——这些是深层加工。如果你能在使用AI的同时保持同样的认知参与度，快不是问题。如果你在用纸笔但只是机械抄写，慢也救不了你。

认知卸载（cognitive offloading）的研究支持这个区分。Grinschgl等人2021年的实验发现，使用外部工具确实削弱了后续记忆——但只在参与者**没有明确的学习目标**时如此。当参与者知道稍后需要回忆这些信息时，即使被强制使用外部工具，他们"几乎完全抵消了卸载对记忆的负面影响"[^10]。

关键变量不是"有没有使用工具"，而是"使用工具时有没有保持主动加工的意图"。

这引出一个不太舒服的推论：**摩擦的必要性和你在该领域的积累成反比。**

对于还在构建基础认知框架的人——小学生、新入门的学习者——外在的、结构性的摩擦几乎是必要的。你不能指望一个8岁的孩子在iPad面前"主动选择深层思考"。瑞典把课本发回小学教室，在这个意义上是对的。

但对于已经在某领域有深厚积累的人，强制保留摩擦就像要求一个经验丰富的木匠放弃电锯改用手锯来"保持对木头的感觉"。不是完全没道理，但收益递减。微软研究的数据正好支持这一点：高自我信心者使用AI后反而投入更多批判性审视。

---

## 真正的问题：从外在约束到自律行为

以前你写一篇文章，摩擦是默认的——你必须坐在那里一个字一个字敲出来，过程天然迫使你思考。现在你可以让AI生成，思考变成了可选项。

这就像食物稀缺的年代，你不需要"意志力"来控制饮食。现在超加工食品无处不在，节制变成了一个纯粹的自律问题。问题不是食物变丰富了，问题是人类的自律能力没有同步升级。

AI的真正危险不是它消除了摩擦，而是它**把"是否进行深层认知参与"从一个你不得不面对的外在约束，变成了一个你必须主动选择的自律行为**。Weizenbaum在1976年就以另一种方式说过类似的话："权力若不是选择的权力，便什么也不是。工具理性可以做出决定，但'决定'和'选择'之间有着天壤之别。"[^11]

Lee等人的研究也印证了这一点：AI使用中批判性思维的三大障碍分别是——意识不足（不知道自己在偷懒）、动力不足（时间压力下觉得"够好就行"）、能力不足（在不熟悉的领域无法有效评估AI输出）[^3]。这三个障碍的共同特征是，它们在没有外在摩擦时会被放大。

---

## 不是回到纸笔，而是重新设计参与

如果我们接受"认知参与才是关键"这个前提，那正确的应对方案就不是"把AI拿走、回到纸笔"——那是在逃避问题。真正的挑战是：**在工具已经存在的前提下，如何设计工作流和习惯，使认知参与不再依赖外在摩擦的强制。**

HCI领域近两年讨论的"friction-in-design"给了一些方向。它不是把整个流程变慢，而是在关键决策点有策略地插入反思节点。Lee等人的研究本身就提到了一些设计策略：将AI的解释以问题而非陈述的形式呈现、要求用户等待后才能看到AI输出、设置注意力检查节点——这些"认知强制函数"（cognitive forcing functions）在实验中显著降低了对AI的过度依赖[^3]。

在个人实践层面，这可能意味着：

- AI生成了一份草稿，你不是直接发出去，而是必须自己写一段"这个方案的核心逻辑是什么"的摘要
- 用AI做了信息检索，你不是直接引用结论，而是必须重新组织它和你已有知识的关系
- 在学习新领域时，刻意保留手写笔记、自己推导的环节
- 在已经熟悉的领域，放心使用AI处理机械性工作

这不是一个关于"科技好还是不好"的争论。它是一个关于**在什么阶段、对什么人、在什么任务上保持什么程度的认知参与**的工程问题。答案不是一刀切的。

---

瑞典把课本发回了教室，但真正拯救瑞典教育的不会是课本。Jakobsson说得很清楚：解决学校隔离才是根本[^6]。同样，拯救我们认知能力的也不会是对AI的回避，而是在一个默认无摩擦的世界里，仍然有意识地选择为自己制造认知挑战——不是因为低效有什么美德，而是因为思考这件事本身就不可能被外包。

Weizenbaum在半个世纪前写道，计算机的引入"仅仅加固和放大了那些先前就存在的压力"[^11]。AI也是一样。它没有创造懒惰，它只是让懒惰的代价变得更隐蔽、更延迟、更难察觉。在以前，不思考的后果是写不出东西。现在，不思考的后果是你写出了一堆看起来不错但不属于你的东西，而你甚至不知道自己损失了什么。

## 参考文献

[^1]: Knutsson, L. (2025). *Sweden Went All in on Screens in Childhood. Now It's Pulling the Plug.* After Babel. [链接](https://www.afterbabel.com/p/sweden-went-all-in-on-screens-in)

[^2]: Bjork, E. L. & Bjork, R. A. (2011). *Making Things Hard on Yourself, But in a Good Way: Creating Desirable Difficulties to Enhance Learning.* 收录于Psychology and the Real World. 原始概念出自 Bjork, R. A. (1994). [链接](https://bjorklab.psych.ucla.edu/wp-content/uploads/sites/13/2016/04/EBjork_RBjork_2011.pdf)

[^3]: Lee, H. P., Sarkar, A., Tankelevitch, L., Drosos, I., Rintel, S., Banks, R. & Wilson, N. (2025). *The Impact of Generative AI on Critical Thinking.* CHI '25. [链接](https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/)

[^4]: Woods, A. (2026). *Don't Let AI Write For You.* [链接](https://alexhwoods.com/dont-let-ai-write-for-you/)

[^5]: OECD (2023). *PISA 2022 Results — Sweden Country Note.* [链接](https://www.oecd.org/en/publications/pisa-2022-results-volume-i-and-ii-country-notes_ed6fbcc5-en/sweden_de351d24-en.html)

[^6]: Malmqvist, M. (2023). *Lack of vision for Swedish schools, an academic reflects on PISA results.* Malmö University. [链接](https://mau.se/en/news/lack-of-vision-for-Swedish-schools-an-academic-reflects-on-PISA-results/)

[^7]: Mueller, P. A. & Oppenheimer, D. M. (2014). *The Pen Is Mightier Than the Keyboard: Advantages of Longhand Over Laptop Note Taking.* Psychological Science, 25(6), 1159-1168.

[^8]: Morehead, K., Dunlosky, J. & Rawson, K. A. (2019). *How Much Mightier Is the Pen than the Keyboard for Note-Taking? A Replication and Extension of Mueller and Oppenheimer (2014).* Educational Psychology Review, 31(3), 753-780. [链接](https://doi.org/10.1007/s10648-019-09468-2)

[^9]: Bjork, R. A. & Bjork, E. L. (2020). *Desirable Difficulties in Theory and Practice.* [链接](https://www.waddesdonschool.com/wp-content/uploads/2021/02/Desriable-Difficulties-in-theory-and-practice-Bjork-Bjork-2020.pdf)

[^10]: Grinschgl, S., Papenmeier, F. & Meyerhoff, H. S. (2021). *Consequences of cognitive offloading: Boosting performance but diminishing memory.* Quarterly Journal of Experimental Psychology, 74(10), 1758-1773. [链接](https://doi.org/10.1177/17470218211008060)

[^11]: Weizenbaum, J. (1976). *Computer Power and Human Reason: From Judgment to Calculation.* W.H. Freeman.
