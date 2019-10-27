---
layout:     post
title:      "《Graph Neural Networks- A Review of Methods and Applications》读书笔记"
subtitle:   "Analysis of the real usage and impact of GitHub reactions"
date:       2019-10-24
author:     "Haoyue"
header-img: "img/post-bg-reactions.jpg"
tags:
    - 读书
    - 生活
---

## 《Graph Neural Networks- A Review of Methods and Applications》读书笔记

这是一篇发表在SBES2019，巴西第三十三届软件工程研讨会论文集中的论文。针对Github在2016年推出的一项新的社交功能——类似于表情符号的**Reactions**，研究了其在软件开发方面的使用情况以及影响，并根据所得结果给出了一些实用性的建议。

## 一、Background
Github推出的Reactions功能可以让开发者们快速地表达自己对于issues和pull requests的感受。Github一共提供了**6**种不同的Reactions，分别是 ***Thumbs up***，***Thumbs down***，***Laugh***，***Hooray***，***Confused***，***Heart***。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_27.png)

其中，
***Thumbs up*** 表示接受或者认可；
***Thumbs down*** 表示拒绝或者不赞成；
***Laugh*** 表达有趣或者高兴，也可以用来表示讽刺和嘲讽；
***Hooray*** 表示庆祝或者高兴；
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

文中只对issues及其相关的评论中的reactions进行了分析。研究人员收集了自2016年3月13日，即reactions功能正式发布后的项目中的issues，一共从4841个Github项目中收集了2544304个issues和9775928条评论，平均每个issue有3.84条评论。并且这些项目由70种不同的编程语言实现。

## 三、Result

#### 问题1：Do developers use reactions?
在文中使用的数据集中，87.0%的issues和89.3%的评论没有使用reactions。但是如果考虑整个issues的过程(包括issues和评论)，发现有28.4%至少含有一个reaction。
MICROSOFT/VSCODE项目中的issue——*Allow for floating windows*拥有最多的reactions(3654个reactions)；至于评论，WEBASSEMBLY/DESIGN项目中的issue——*#980 – WebAssembly logo voting*的评论中的reactions数目最多。
此外，研究者们还发现，使用C++语言实现的项目获得的reactions最多，Objective-C语言实现的项目获得的reactions最少，猜测原因是iOS开发正转向Swift，因此，Objective-C项目可能会被弃用。

#### 问题2：What are the most common reactions?
***Thumbs up*** 是最常用的reaction(78.7%)，其次是 ***Hooray*** 和 ***Heart***，两者的占比均为5.9%，然后是 ***Thumbs down***(5.0%)。***Confused*** 是最不常用的reaction(1.7%)。

#### 问题3：How is the usage of reactions evolving?
自从GitHub推出reactions以来，reactions的使用在不断增加。
在2018年，开发者使用的reactions比2017年增加了32.5％。相比之下，自2016年以来，每个月的新issues的数量几乎保持不变。至于每种reaction类型的发展，发现现有的每个reaction的发展方式没有很大区别。

#### 问题4：Do popular projects have more reactions?
项目的受欢迎程度与其获得的reactions的数量之间基本没有关联关系。

#### 问题5：Do certain types of issues have more reactions?
文中针对八个类型的issues进行了分析，分别是*bug*，*duplicate*，*enhancement*，*invalid*，*question*，*wontfix*，*help*和*stale*，其中 ***bug*** 是最常见的issues类型。
当只考虑*bug*和*enhancement*的时候，***enhancement*** 类型的issues经常获得reactions，每个*bug*获得的reactions的数量(0.35)比每个*enhancement*获得的reactions的数量(1.39)低四倍。
而且，issue和reaction的类型之间存在关联。*Thumbs down*，*Laugh*和*Confused*在无效(*invalid*)的issues中最为常见。

#### 问题6：Do issues with more reactions get resolved faster?
针对该问题，文中分析了*bug*和*enhancement*类型的issues，并且只分析至少含有102个issues的仓库。
含有reactions的*bug*和*enhancement*类型的issues需要更长的时间才能关闭，猜测是因为这些issues比较复杂，需要开发者更多的时间去解决。

#### 问题7：Do issues with more reactions have more discussion?
文中比较了*bug*和*enhancement*类型中没有reactions和至少含有两个reactions的issues中讨论的数量。含有reactions的*bug*和*enhancement*类型的issues需要更长时间的讨论。
因此，reactions倾向于出现在需要进行更多讨论的相对复杂的issues中。

#### 问题8：Do negative reactions inhibit further participation?
大多数开发者收到的负面reactions很少，而其他开发者可能收到很多。与其他社交平台不同，负面reactions不会影响GitHub的开发者参与进某一仓库，并进行下一步的开发。

#### 问题9：Do reactions reduce the noise in issue conversations?
Reactions没有减少评论中表情符号的使用，但是评论中只使用一个*Thumbs up*表情符号的现象大大减少。

## 四、Practical Recommendation
1. 使用reactions可以表达修复bugs或者实现新功能的重要性。
2. 使用reactions可以进行用户调查。
3. 在发行新版本之前，可以使用reactions收集用户的反馈。
4. 使用reactions可以表明一个无效的issue。
5. 与*bug*类型的issues相比，用户经常对*enhancement*类型的issues做出reactions。这可能揭示了GitHub维护者需要不断开发新功能来发展其系统的压力。
6. GitHub用户不应该期望通过对issues做出reactions，使得issues在几天之内得到处理。
7. GitHub用户不应该将reactions视为能够加速修复bugs或者实现新功能的工具。
8. Reactions不应该给开发者带来压力，它只提供反馈。
9.  GitHub开发者不应该因为负面reactions而放弃项目。
10. GitHub应该考虑支持其他表情符号，例如*Clapping hands*。
11. 允许GitHub开发者突出显示issues或者评论的句子，然后仅在突出显示的文本中使用reactions。