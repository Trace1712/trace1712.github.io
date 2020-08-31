---
layout:     post
title:      2020-08-24-Recent-Advances-in-Hierarchical-Reinforcement-Learning
subtitle:   paper
date:       2020-08-23
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning paper
---


# <center>论文笔记</center>

**摘要：**强化学习因为维度诅咒，使得参数爆炸式增长，最近解决这个问题的方法转变从根本上改变寻找方式入手，不需要每一步都作决策。

这引出了分级控制和相关的学习算法。我们回顾了几种最近发展的机器学习算法和时间抽象。通常都是使用半马尔可夫模型。

我们之后讨论了这种想法对于当前实验，多智能体协同和分级存储对解决部分可观测性的扩展。

**结论：**我们写这章的目的是回顾几种最近在机器学习方面的，相关的时间序列抽象和分级控制算法。萨顿的期权形式主义，抽象机器方法的层次结构（HAMS），和MAXQ框架。我们也探讨了这些方法解决现实问题的扩展，多智能体协同，和可观测性扩展的分级存储。我们希望这文章会证明在推进需要的会议中是有用的。

# 1.马尔可夫和半马尔可夫

大部分强化学习都基于马尔可夫过程的形式。尽管RL决不受限于(restrict)马尔可夫过程，离散计数，课书的状态动作提供了在学习基础算法中最简单的学习框架。

在一系列阶段中的每个状态里，一个智能体（控制者）观察系统的状态s，包含有限的集合S，从一个有限，不空的集合中选取一个合法的动作As。这个之呢个体收到了即使奖励R(s,a)，下一个状态是概率为P(s<sup>'</sup>|s,a)下的s<sup>'</sup>。

可以定义状态价值函数V
$$
V^{\pi}(s) = E\{r_{t+1}+\gamma r_{t+2}+\gamma^2 r_{t+3}+\dots|s_t=s,\pi\} 0 \le \gamma<1
$$
这是一个有限，无限水平，有折扣的MDP。这个的目标是为了寻找一个优化策略V<sup>*</sup>是价值函数符合其他任意优化策略。大部分RL通过压缩最简单的MDP类来解决折扣MDP。

RL算法中扮演重要角色的是动作价值函数，从课接受的动作-价值队中分配值。给定一个策略&pi;，（s,a）上的价值为Q。
$$
Q^{\pi}(s,a)=E\{r_{t+1}+\gamma r_{t+2}+\gamma^2r_{t+3}+\dots|s_t=s,a_t=a,\pi\}
$$
优化动作价值函数Q<sup>*</sup>,分配给每个可接受的动作价值对(s,a)，期望的水平折扣返回s中的可执行的动作a。动作-价值函数相似于其他的定义的返回。

DP算法发现动作函数适合于各种贝尔曼方程如
$$
V^\pi(s)=\sum_{a\in\cal A_s}\pi(s,a)[R(s,a)+\gamma\sum_{s^{'}}P(s'|s,a)V^{\pi}(s')]\\
V^*(s)=\max _{a\in A_s}[R(s,a)+\gamma \sum_{s'}P(s'|s,a)V^*(s')]
$$
对所有状态s中的S。相似的等式存在Q函数
$$
Q^*(s,a)=R(s,a)+\gamma\sum_{s'}P(s'|s,a)\max_{a'\in A_{s'}}Q^*(s',a')
$$
对于所有的s,a。拿DP算法做个例子，考虑到价值迭代。对于每个item k,更新近似值V<sub>k</sub>,对每个状态s,用下面地等式
$$
V_{k+1}(s)=\max_{a\in A_{s}}[R(s,a)+\gamma\sum_{s'}P(s'|s,a)V_{k}(s')]
$$
我们称这个操作是一个备份，因为它通过向它传递有关其可能继承状态的近似值的信息来更新一个状态的值。用这些备份操作来更新每个值叫一个sweep。一个相似算法使用下面地备份成功地估计Q
$$
Q_{k+1}(s,a)=R(s,a)+\gamma\sum_{s'\in S}P(s'|s,a)\max_{a'\in A_{s'}}Q_k(s',a')
$$
在一个MDP中，只有决策过程的连续性才是相关的，而不是决策阶段之间经过的时间量。这个地概述就是半马尔可夫决策过程（SMDP），是在一个决定和下一个随机变量之间。在真实的价值案例中，SMDPS模型是连续时间离散事件系统模型。一个离散时间的SMDP决策只在整数的基本时间步长上。无论什么情况，它通常用来使得系统记住随机等待时间的每个状态的剩余。

因为它的相关性非常弱，离散时间SMDP形式构成了大部分分级RL的方法，但这里没有明显的障碍来扩展这些方法到连续时间的案列。

假设随机变量&tau;表示状态s的等待时间，当动作已经被执行。转移概率提供了一个动作a被执行，&tau;步后从s到s'的连接概率。我们把这个概率写作P（s', &tau; |s,a）。这和期望的即使奖励R(s,a)，现在给与了在s状态，等待时间上折扣极了。贝尔曼方程变成了这样
$$
V^*(s)=\max_{a\in A_{s}}[R(s,a)+\sum_{s',\tau}\gamma^{\tau}P(s',\tau|s,a)V^*(s')]\\
对所有s\in S\\
Q^*(s,a)=R(s,a)+\sum_{s',\tau}\gamma^{\tau}P(',\tau|s,a)\max_{a'\in A_{s'}}Q^*(s',a')
$$

# 2.强化学习

略

# 3.分级强化学习的方法

最简单的抽象形式的一个叫宏指令的操作。是一系列的操作或者动作被他的名字包装，看上去好像是一个操作。宏规定了基本的分层操作的描述或者系列动作。大部分现在的分层强化学习研究专注于动作分级，他们遵循与宏或子程序层次结构大致相同的语义。

从一个控制角度，一个宏是一个开放层次控制策略，比如对大多数有趣的控制目标是不恰当的，特别是控制随机系统。RL的分层方法概括为闭环策略的宏方法或者更加精确，闭环策略因为他们通常被定义为一个状态集合的子集。这些部分策略又是被成为暂时扩展行动，技巧，行为，或者更多的控制理论模式。当不讨论一个详细的形式，我们会使用term activity。

11页
