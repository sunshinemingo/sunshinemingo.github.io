---
layout:     post
title:      "《A Large Scale Study of Long-Time Contributor Prediction for GitHub Projects》读书笔记"
subtitle:   "How can a newcomer be a long time contributors for a open sorce project"
date:       2020-03-16
author:     "Haoyue"
header-img: "img/post-bg-lagre-scale.jpg"
tags:
    - 读书
    - 生活
    - 学习
---

## 《A Large Scale Study of Long-Time Contributor Prediction for GitHub Projects》读书笔记
长期贡献者（LTC）的持续贡献是使开源软件（OSS）项目成功和生存的关键因素。在本文中，我们从 GHTorrent 收集 GitHub 数据，用于预测 OSS 项目中的新人是否会成为LTC。

文中选择了最受欢迎的917个项目，其中包含75046个贡献者。如果开发者在项目中的第一次提交和最后一次提交之间的时间间隔大于特定时间 T，则认为他是项目的 LTC。实验中使用了三种不同的时间间隔：1年、2年和3年。在三种时间间隔设置中，分别有9238、3968和1577位贡献者成为了项目的LTC。

## 一、INTRODUCTION
开源项目可以有许多贡献者，但是只有一小部分贡献者通常会在项目中停留很长时间。这些长期贡献者（LTC）通常贡献大量代码。他们通常也是经验丰富的开发者，具有丰富的项目经验，在项目成功中扮演着重要的角色。他们的贡献不仅涉及代码编写，还涉及其他任务，例如解决错误报告，执行代码审查，理解用户的要求以及帮助和鼓励新来者。如果开源项目能够吸引有才能的开发者并将其保留为LTC，则它更有可能取得成功。然而，一个开源项目中的大多数贡献者都离开了该项目，并没有成为LTC。如果可以尽早地发现潜在的 LTC，项目维护者可以采取一些措施来尽可能地保留LTC。

文中通过基于新来者第一个月的开发数据构建预测模型，预测是否会成为长期贡献者（LTC）。对于每个项目的每个贡献者，基于GitHub数据提取了五个维度的特征：**developer profile**，**repository profile**，**developer monthly activity**，**repository monthly activity** 和 **collaboration network**。

基于这些提取的特征，研究了四个问题：
1、能否有效地预测开发者在提交其对项目的首次 commit 后不久将成为该项目的长期贡献者？
2、与仅基于部分特征构建的预测模型相比，基于所有特征构建的预测模型的效果如何？
3、哪些特征对于判断开发者成为长期贡献者最重要？
4、文中提出的方法在跨项目，跨编程语言和跨项目规模预测中的效果如何？


## 二、EXPERIMENT SETUP

### (1)Dataset
实验中使用的数据来自 GHTorrent，首先根据项目的 star 数选择前1000个项目，同时排除以下项目：

* GHTorrent数据库中项目的编程语言为空。
* GITHUB未被用作 issue 跟踪系统。
* 该项目是从另一个项目fork而来的。
* 该项目已从GitHub中删除。

最终得到了971个项目。对于每个项目，我们考虑了至少提交了一次commit的开发者。有些开发者在项目中的首次提交时间可能早于其GitHub帐户的创建时间，我们会排除这些开发者。此外，我们还排除了在数据收集时间内加入的开发者，因为无法确定他是否成为项目的 LTC。

文中将长期贡献者（LTC）定义为在项目中停留超过特定时间T的贡献者，即贡献者对该项目的第一次提交和最后一次提交之间的时间间隔大于T。实验中设置了三种不同的时间间隔设置：1年、2年和3年。

### (2)Studied Features
实验中提取了5个维度、共63个可能影响新来者成为 LTC 的特征。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_43.png)

**Developer Profile**
* user_age：从新开发者的注册日期到他/她加入 repo 的日期之间的天数；
* user_own_repos：新开发者加入该 repo 时拥有的 repo 的数量；
* user_watch_repos：新开发者加入该 repo 时关注的 repo 的数量；
* user_contribute_repos：新开发者加入该 repo 后已提交至少一次 commit 的 repo 的数量；
* user_history_commits：新开发者加入该 repo 时已经提交的 commit 的次数；
* user_history_issues：新开发者加入 repo 时已经提交的 issue 的数量；
* user_history_pull_requests：新开发者加入该 repo 时已提交的 PR 的数量；
* user_history_followers：关注新开发者的用户数。
  
**Repository Profile**
* language：repo 使用的主要编程语言；
* before_repo_commits：新开发者加入时，repo 具有的 commit 的次数；
* before_repo_commit_comments：新开发者加入时，提交到 repo 的 commit 的comment 的数量；
* before_repo_contributors：新开发者加入时，repo 拥有的贡献者数量；
* before_repo_contributor_{S}：repo 中贡献者提交的统计信息，其中S可以是最大值，最小值，均值，中位数和标准差；
* before_repo issues：新开发者加入时，repo 中所提出的 issue 的数量；
* before_repo_issue_comments：新开发者加入时，repo 中所提出的 issue 的 comment 的数量；
* before_repo_issue_events：新开发者加入时，repo 中的 issue events 的数量；
* before_repo_issue_events_closed：新开发者加入时，repo 中关闭的 issue events 的数量；
* before_repo_issue_events_assigned：新开发者加入时，repo 中已分配的 issue events 的数量；
* before_repo_pull_requests：新开发者加入时，repo 中 PR 的数量；
* before_repo_pull_request_comments：新开发者加入时，repo 中 PR 的 comment 的数量；
* before_repo_pull_request_history：新开发者加入时，repo 中已有的 PR event 的数量；
* before_repo_pull_request_history_merged：新开发者加入时，repo 中已经合并的 PR 的数量；
* before_repo_pull_request_history_closed：新开发者加入时，repo 中已经关闭的 PR 的数量；
* before_repo_watchers：新开发者加入时，repo 拥有的关注者的数量。

**Developer Monthly Activity**
* month_user_commits：新开发者在第一个月提交到 repo 的 commit 的数量；
* month_user_commit_comments：新开发者在第一个月提交到 repo 的 commit 收到的 comment 的数量;
* month_user_issues：新开发者第一个月在 repo 中提出的 issue 的数量;
* month_user_issue_comments：新开发者第一个月在 repo 中提出的 issue 收到的 comment 的数量;
* month_user_issue_events：新开发者第一个月在 repo 中提出的 issue 收到的 event 的数量;
* month_user_issue_events_closed：新开发者第一个月在 repo 中提出的 issue 已经关闭的 event 的数量;
* month_user_issue_events_assigned：新开发者第一个月在 repo 中提出的 issue 已经分配的 event 的数量;
* month_user_pull_requests：新开发者第一个月在 repo 中提交的 PR 的数量；
* month_user_pull_request_comments：新开发者第一个月在 repo 中提交的 PR 收到的 comment 的数量；
* month_user_pull_request_history：新开发者第一个月在 repo 中提交的 PR 收到的 event 的数量；
* month_user_pull_request_history_merged：新开发者第一个月在 repo 中提交的 PR 已经被合并的数量；
* month_user_pull_request_history_closed：新开发者第一个月在 repo 中提交的 PR 已经被合并的数量。

**Repository Monthly Activity**
* month_repo_commits：新开发者加入 repo 后第一个月提交到 repo 的 commit 的数量；
* month_repo_commit_comments：第一个月提交给 repo 的 commit 收到的 comment 的数量；
* month_repo_contributors：第一个月至少提交了一次 commit 到 repo 的贡献者的数量；
* month_repo_contributor_{S}：第一个月的贡献者的 commit 的统计信息，其中S可以是最大值，最小值，均值，中位数和标准差；
* month_repo_issues：第一个月提出的 issue 的数量；
* month_repo_issue_comments：第一个月提出的 issue 收到的 comment 的数量；
* month_repo_issue_events：第一个月提出的 issue 收到的 event 的数量；
* month_repo_issue_events_closed：第一个月提出的 issue 已经关闭的 event 的数量；
* month_repo_issue_events_assigned：第一个月提出的 issue 已经分配的 event 的数量；
* month_repo_pull_requests：第一个月提交到 repo 的 PR 的数量；
* month_repo_pull_request_comments：第一个月提交到 repo 的 PR 收到的 comment 的数量；
* month_repo_pull_request_history：第一个月提交到 repo 的 PR 收到的 event 的数量；
* month_repo_pull_request_history_merged：第一个月提交到 repo 的 PR 已经被合并的数量；
* month_repo_pull_request_history_closed：第一个月提交到 repo 的 PR 已经被关闭的数量。

**Collaboration Network**
这些指标用于量化OSS项目协作结构中新来者的活跃程度。在协作结构图中，每个节点表示一个开发者，对于每次有关 commit / issue / pull request 的讨论，就创建一个从每个参与者到其创建者的有向边。
* degree_centrality：入射到节点上的链接数（即节点具有的联系数）；
* closeness_centrality：一个节点到所有其他节点的所有距离之和的倒数，它可以量化节点与网络中所有其他节点的接近程度；
* betweenness_centrality：连接到节点的三角形数量与以该节点为中心的三元组数量之间的比率，其中以该节点为中心的三元组是连接到该节点的两个边的集合；
* eigenvector_centrality：通过该节点的所有可能的节点对之间的最短路径总数；
* clustering_coefficient：一种网络度量，它基于连接到高度中心性节点会增加节点中心性的概念，将分数分配给网络中的节点。

### (3)Prediction Model
实验中使用了不同的分类器，包括朴素贝叶斯，支持向量机（SVM），决策树，K近邻（kNN）和随机森林。

### (4)Evaluation Metrics
实验中使用AUC，即 receiver operating characteristic（ROC）曲线下的面积，来评估所提出的预测模型的有效性。通过在所有阈值上绘制真阳率（TPR）与假阳率（FPR）来创建ROC曲线。AUC的值介于0到1之间。AUC值越高，分类器的性能越好。 

## 三、EXPERIMENT RESULTS
**问题1 能否有效地预测开发者在提交其对项目的首次 commit 后不久将成为该项目的长期贡献者？**

实验中使用Weka工具进行实验。将917个项目的所有开发者合并为一个整体数据集。这些开发者首先按照其加入项目的时间顺序进行排序，然后将列表分为10个大小相同的不重叠窗口。将前n个窗口用作训练数据，将其余的10-n个窗口用作测试数据（n从1到9）。最后通过AUC评估效果。实验中使用的分类器的参数设置为最优参数。此外，实验室选择随机模型作为基线模型，与其他预测模型进行比较。

实验结果表明，随机森林的预测效果最好。为了证实这一点，实验中使用带有 Bonferroni 校正的 Wilcoxon 符号秩检验来研究随机森林与其他预测模型相比，检测效果是否提升，同时还使用 Cliff 的增量来衡量效果大小。

**问题2 与仅基于部分特征构建的预测模型相比，基于所有特征构建的预测模型的效果如何？**

对于每个维度，有两个实验设置：一个是仅基于某一维度的特征构建随机森林模型，另一个是基于其他四个维度中的特征构建随机森林模型，并且使用与问题1相同的设置来评估预测模型的性能，结果中的 AUC 是9轮验证的平均值。然后，将基于所有特征构建的随机森林模型的AUC与基于特征子集构建的模型的AUC进行比较。之后使用带有 Bonferroni 校正的 Wilcoxon 符号秩检验来研究在这五个模型上基于所有特征构建的随机森林的改进是否具有统计显著性，还使用Cliff的增量来衡量效果大小。对于 LTC 预测的每个时间间隔，都会重复此过程。

实验结果表明，综合五个维度的特征，有利于提高随机森林模型的有效性。

**问题3 哪些特征对于判断开发者成为长期贡献者最重要？**

实验中仅考虑随机森林模型中最重要的特征。使用相关性分析和冗余性分析后，对于 LTC 预测的不同时间间隔（即1、2和3年），分别剩下40、40和38个特征。为了评估特征的重要性，使用 10-fold cross-validation 来评估模型的有效性，为了避免实验的随机性，建立并运行了1000次预测模型。之后，所有1000次运行的重要性值应用 ScottKnott ESD 测试，以确定对于整个数据集最重要的特征。

为了了解这些功能的影响，使用 Wilcoxon 秩和检验和 Bonferroni 校正来分析 LTC 和非 LTC 开发者之间差异的统计显著性，还使用 Cliff 的增量来衡量两组之间差异的影响大小。

实验结果表明，在这三个时间间隔设置中，有八个特征属于十大最重要的特征，分别是**user_history followers**，**user age**，**language**，
**before repo watchers**，**before repo contributor mean**，**before repo commits**，**month user commits** 和 **month repo commits**。

**问题4 文中提出的方法在跨项目，跨编程语言和跨项目规模预测中的效果如何？**

对于每个项目，我们首先根据其数据构建一个预测模型，然后使用该模型预测其他项目中开发人员的标签。在跨项目预测设置中，仅使用拥有500名以上开发人员的项目。最终得到21个具有不同编程语言和贡献者数量的项目。对于这21个项目，实验中还基于整个数据中所有其他项目的数据构建了一个预测模型，然后使用该模型预测该项目中开发人员的标签。仅使用随机森林来构建预测模型，因为实验证明它在问题1中具有最佳性能。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_44.png)

对于 LTC 预测的跨语言设置，给定一种编程语言，基于所有项目构建一个预测模型，将其作为主要编程语言，然后使用该模型预测使用另一种语言的项目中开发者的标签。同样还基于使用所有其他语言的项目构建一个预测模型，然后使用该模型预测使用其余语言的项目中开发者的标签。在此设置中，选择了12种编程语言，它们具有1000多个实例。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_45.png)

对于用于 LTC 预测的跨项目大小的实验，首先按贡献者的数量对所有项目进行排序，然后使用分位数将它们分为四个相等大小的组。 这四个组分别表示为Q1，Q2，Q3，Q4。 然后，针对每个小组，基于其中的项目构建一个预测模型，并使用该模型来预测其他小组的项目中开发者的标签。同样还基于所有其他组的项目构建一个预测模型，然后使用该模型预测其余项目中开发人员的标签。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_46.png)


## 四、FEEDBACK FROM DEVELOPERS

为了调查开发者如何参与 OSS 项目以及他们对影响开发者成为 LTC 的因素的看法，我们进行了一项调查。该调查的主要目的是验证 OSS 开发者是否认为文中方法提出的重要特征对开发者是否成为LTC有影响。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_47.png)

我们从数据集中的75046名开发者中随机抽取了1250个人。其中，以3年为时间间隔选择了250名LTC，以1年为时间间隔从非LTC中选择了1000名。在这些接收者中，我们分别收到了来自LTC的26条答复和来自非LTC的122条答复，其总体答复率为11.84％。

对于文中提出的三个重要特征，分别有58％，64％和62％的受访者同意或强烈同意，认为它们对开发者成为 LTC 有重大影响。

此外，从收集到的问卷中，还确定了三类可能影响新来者成为 LTC 的因子，分别是**project quality**，**project community**，**personal factors**。

## 五、DISCUSSION

文中提出的预测模型与基于 Bug Reports 的 LTC 预测方法进行了比较，发现有关开发者和项目的更多信息可以改善预测模型的性能。但是，如果贡献者只是GitHub的新人，而一个项目几乎没有历史活动，那么文中的方法将与基于 Bug Reports 的 LTC 预测方法具有相似的性能。

接着，通过实验来判断 **10-fold Cross-Validation**、**Developer Productivity**、**Feature Selection** 和 **Project Popularity** 对文中提出方法的性能是否会有影响，结果发现这些因素对实验结果都没有太大的影响。

之后，基于实验的分析结果以及调查问卷的结果，对 OSS 项目给出了一些建议：
* 留住有兴趣且经验丰富的开发者；
* 特别鼓励新来者在第一个月多做贡献；
* 提高项目社区的积极性；
* 不同的留住新来者的策略可能适合不同语言开发的项目；
* 在不同开发阶段的项目应使用不同的策略来留住新来者。

## 六、CONCLUSION

基于 GitHub 软件开发中的多种数据，使用数据挖掘技术来判断新的开发者是否会成为项目的 LTC。实验数据基于 GHTorrent 构建，包含917个项目。针对三种不同的时间间隔设置，1年、2年、3年，分别获得了9238、3968和1577个成为LTC的开发者。研究表明，在三个时间间隔设置中，最有效的分类器是随机森林，最重要的特征是关注者的数量。此外，研究还发现，新的开发者加入项目时，项目的编程语言和其他开发者贡献的平均次数也属于 LTC 预测的前10个最重要的特征。
