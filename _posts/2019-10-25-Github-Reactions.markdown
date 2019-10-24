---
layout:     post
title:      "《Beyond Textual Issues: Understanding the Usage and Impact of GitHub Reactions》读书笔记"
subtitle:   "Analysis of the real usage and impact of GitHub reactions"
date:       2019-10-25
author:     "Haoyue"
header-img: "img/post-bg-reactions.jpg"
tags:
    - 读书
    - 生活
---

## 《Beyond Textual Issues: Understanding the Usage and Impact of GitHub Reactions》读书笔记

这是一篇发表在SBES2019，巴西第三十三届软件工程研讨会论文集中的论文。针对Github在2016年推出的一项新的社交功能——类似于表情符号的**Reactions**，研究了其在软件开发方面的使用情况以及影响，并根据所得结果给出了一些实用性的建议。

## 一、Background
Github推出的Reactions功能可以让开发者们快速地表达自己对于issues和pull request的感受。Github一共提供了**6**种不同的Reactions，分别是 ***Thumbs up***，***Thumbs down***，***Laugh***，***Hooray***，***Confused***，***Heart***。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_27.png)

其中，
***Thumbs up*** 表示接受或者认可；
***Thumbs down*** 表示拒绝或者不赞成；
***Laugh*** 表达有趣或者高兴，也可以用来表示讽刺和嘲讽；
***Hooray*** 表示庆祝或者表达高兴；
***Confused*** 表达对于某一特定主题感到困惑或者在理解上有困难；
***Heart*** 表达强烈的接受或者赞成。

## 二、Study Design
文中从**Reactions的使用**和**Reactions的影响**两方面，共**9**个问题进行了调查研究。其中，Reactions的使用方面包含了5个问题，Reactions的影响方面包含了4个问题。
###### Reactions的使用：
问题1：Do developers use reactions?
问题2：What are the most common reactions?
问题3：How is the usage of reactions evolving?
问题4：Do popular projects have more reactions?
问题5：Do certain types of issues have more reactions?
###### Reactions的影响：
问题6：Do issues with more reactions get resolved faster?
问题7：Do issues with more reactions have more discussion?
问题8：Do negative reactions inhibit further participation? 
问题9：Do reactions reduce the noise in issue discussions?

文中只对issues及其相关的评论中的Reactions进行了分析。研究人员收集了2016年3月13日，即Reactions功能正式发布后的项目中的issues，一共从4841个Github项目中收集了2544304个issues和9775928条评论，平均每个issue有3.84条评论。并且这些项目由70种不同的编程语言实现。

## 三、Result
##### 问题1：Do developers use reactions?
在文中使用的数据集中，87.0%的issues和89.3%的评论没有使用reactions。但是如果考虑整个issues的过程，发现28.4%的至少有一个reaction。
MICROSOFT/VSCODE项目中的issue——*Allow for floating windows*拥有最多的reactions(3654个reactions)；至于评论，WEBASSEMBLY/DESIGN项目中的issue——*#980 – WebAssembly logo voting*的评论中的reactions最多。
此外，研究者们还发现，使用C++语言实现的项目获得的reactions最多，Objective-C语言实现的项目获得的reactions最少，猜测原因是iOS开发正转向Swift，因此，Objective-C项目可能会被弃用。

##### 问题2：What are the most common reactions?
***Thumbs up*** 是最常用的reaction(78.7%)，其次是 ***Hooray*** 和 ***Heart***，两者的占比均为5。9%，然后是 ***Thumbs down***(5.0%)。***Confused*** 是最不常用的reaction(1.7%)。

##### 问题3：How is the usage of reactions evolving?
自从GitHub引入此功能以来，将问题数量与反应数量进行了比较。
反应正在增强。 在2018年，开发人员使用的反应比2017年增加了32.5％。相比之下，自2016年以来，每月的新问题数量几乎保持不变。
至于每种反应类型的生长，在现有反应的生长方式上没有主要区别。

Compared the number of issues opened with the number of reactions since this feature was introduced in GitHub.
Reactions are gaining momentum; in 2018, developers used 32.5% more reactions than in 2017. By contrast, the number of new issues per month remains almost constant since 2016.
As for the growth of each reaction type, there is no major difference on the growth pattern of existing reactions.

##### 问题4：Do popular projects have more reactions?**
项目的受欢迎程度与其获得的reactions的数量之间基本没有关联关系。

##### 问题5：Do certain types of issues have more reactions?
文中针对八个类型的issues进行了分析，分别是 ***bug***，***duplicate***，***enhancement***，***invalid***，***question***，***wontfix***，***help*** 和 ***stale***。

Restrict the analysis to eight main labels: bug, duplicate, enhancement, invalid, question, wontfix, help, and stale.
Developers react more often to enhancements. When we consider only bugs and enhancements with reactions, the number of reactions per bug (0.35) is almost four time lower than the number of reactions per enhancements (1.39). Moreover, there is a correlation between issue and reaction type. For example, Thumbs down, Laugh and Confused are more common in invalid issues.

##### 问题6：Do issues with more reactions get resolved faster?
针对该问题，文中分析了 ***bugs*** 和 ***enhancement*** ，并将分析限制在至少含有102个issues的仓库中。
含有reactions的 ***bugs*** 和 ***enhancement*** 的issues需要更长的时候才能关闭，猜测是因为这些issues是比较复杂的，需要开发人员更多的时间去解决。

##### 问题7：Do issues with more reactions have more discussion?
计算bug和增强功能中的注释数量，并比较没有反应且至少有两个反应的问题的结果。
错误和反应的增强功能的讨论时间更长，从本质上确认了RQ＃6的结果，即反应趋于出现在更复杂的问题中，需要进行更多讨论。

Compute the number of comments in bugs and enhancements and compare the results for issues without reactions and with at least two reactions.
Bugs and enhancements with reactions have longer discussion, which essentially confirms the result of RQ #6, i.e., reactions tend to appear in more complex issues, which demand more discussion.

##### 问题8：Do negative reactions inhibit further participation?
大多数开发人员收到的负面反馈很少，其他开发人员收到的反馈也很多。
与其他社交平台不同，负面反馈不会影响GitHub的开发人员参与同一仓库的进一步开发。

Compute the number of interactions performed by developers before and after receiving negative reactions and restrict analysis to issues and comments that have more Thumbs down than the sum of other reactions.
Most developers receive a low number of negative feedback, other developers receive a considerable number of Thumbs down.
Differently from other social platforms, negative reactions do not inhibit GitHub developers participation in further issues of the same repository.

##### 问题9：Do reactions reduce the noise in issue conversations?

Reactions没有减少评论中表情符号的使用，但是评论中只使用一个*Thumbs up*表情符号的现象急剧减少。

Analyzed the issues created after the introduction of reactions and that have emojis in their body.
GitHub reactions did not reduce the usage of emojis in the body of comments.
As for the comments containing only a single emoji, however they are no longer commenting with a single Thumbs up.



## 四、Practical Recommendation
1. 使用反应来表达修复错误或实施新功能的重要性。
2. 使用反应来与用户进行调查。
3. 在部署新版本之前，使用反应收集用户对新版本候选的反馈。
4. 使用反应表明问题无效。
5. 用户对增强的反应比对错误的反应高四倍。这一发现可能揭示了GitHub维护者通过提供新功能而不断发展其系统的压力。
6. GitHub用户不应期望通过对问题做出反应而在几天之内得到处理。
7. 用户不应将反应视为能够加速错误修复或新功能实施的工具。
8. 反应不是压力的工具；而是提供反馈的工具。
9. GitHub开发人员不应将负面反应视为放弃项目的邀请。
10. GitHub应该考虑支持其他表情符号；例如拍手。
11.允许开发人员突出显示问题或评论的特定句子，然后仅在突出显示的文本中应用反应。

1. Use reactions to express the importance of fixing a bug or implementing a new feature.
2. Use reactions to implement surveys with users.
3. Use reactions to collect user’s feedback about a new release candidate, before it is deployed.
4. Use reactions to indicate that an issue is invalid.
5. Users react four times more to enhancements than to bugs. This finding may reveal the pressure suffered by GitHub maintainers to continuously evolve their systems, by providing new features.
6. GitHub users should not expect that by reacting to an issue they will get it handled in a few days. 
7. Reactions should not be viewed by users as an instrument capable of accelerating the implementation of bug fixes or new features. 
8. Reactions are not an instrument of pressure; but an instrument to provide feedback.
9. GitHub developers should not view negative reactions as an invitation to abandon projects.
10. GitHub should consider supporting other emojis; for example Clapping hands.
11. Allow developers to highlight particular sentences of an issue or comment, and then apply a reaction only in the highlighted text.




