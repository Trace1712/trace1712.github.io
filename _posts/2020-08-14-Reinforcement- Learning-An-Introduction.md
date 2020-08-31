---
layout:     post
title:      总结
subtitle:   Reinforcement- Learning-An-Introduction
date:       2020-08-14
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning
---


# <center>第八章</center>

### Exercise8.1

**图8.3中的无规划方法看起来特备差，这是因为它是单步法。使用多步自举法会得到更好的结果。你认为第七章中的多步自举法可以和Dyna方法一样好吗？解释为什么是或者为什么不是？**

Dyna用之前所有的经验来进行优化策略，n步自举法只用最后n步会慢点。

### Exercise8.2

**为什么带额外试探收益的Dyna智能体（Dyna-Q+）会再“屏障迷宫”和”捷径迷宫“任务的第一和第二阶段都表现比原来Dyna-Q智能体好？**

额外试探意味着Dyna-Q+会比Dyna-Q更快发现优化策略。

### Exercise8.3

**仔细观察图8.5可以发现，Dyna-Q+和Dyna-Q的差距在第一阶段推进过程中有微小的收缩。为什么？**

Dyna-Q+将采取次优措施进行探索（当T变大时）。Dyna-Q不会

### Exercise8.5

**如何修改上文展示的表格型Dyna-Q算法使得它能够处理随机的环境？这一修改使得在一变化环境中有多大可能会表现得非常差？如何修改算法使得它能同时适应随机得环境和变化环境。**

1.可以获取转换发生的频率来估计模型的转换模型（MLE估值）

2.计划时进行预测更新

3.如果环境发生变化，这会带来一个问题，因为这些变化只会反映在不断变化的过渡概率上（这可能要很长时间才能反映环境的变化）

4.解决这个问题方法使用探索奖励来估计智能体继续选择不同的状态并保护模型的最新状态