---
layout:     post
title:      Reinforcement- Learning-An-Introduction
subtitle:   第七章
date:       2020-08-04
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning
---

## Reinforcement Learning An Introduction

### <center>第七章</center>

### Exercise7.1

**在第六章，我们注意到，如果价值函数的估计值不是每步都更新，则蒙特卡洛误差可以被改写成时序差分分析（式6.6）的和。请推广该结论。即将式子（7.2）中的n步误差也改写为时序差分误差之和的形式（同样假设在此过程在价值估计不更新)。**
$$
\begin{align}
G_t-V(S_t)&=R_{t+1}+\gamma G_{t+1}-V(S_t)+\gamma V(S_t)+\gamma V(S_{t+1})-\gamma V(S_{t+1})\\

&=\sum_{k=t}^{T-1}\gamma^{k-t}\delta_{k} \tag{6.6}
\end{align}
$$

$$
\begin{align}
V_{t+n}(S_t)=V_{t+n-1}(S_{t})+\alpha[G_{t:t+n}-V_{t+n-1}(S_t)],0\le t\le T\tag{7.2}
\end{align}
$$

$$
\begin{eqnarray}
&G_{t:t+n}=\sum_{i=1}^{n}\gamma^{i-1}R_{t-i}+\gamma^{n}V(S_{t+n})-V(S_t)\\
TD误差& \\
&\delta_t=R_{t+1}+\gamma V(S_{t+1})-V(S_{t})\\
n步误差可以写为&\\
&G_{t:t+n}-V(S_{t})=R_{t+1}+\gamma\sum_{i=1}^{n-1}\gamma^{i-1}R_{t+1+i}+\gamma^nV(S_{t+n})-V(S_t)\\
&G_{t:t+n}-V(S_{t})=\delta_{t}+\gamma(G_{t+1:t+n}-V(S_{t+1}))
\end{eqnarray}
$$

### Exercise7.3

**你认为使用一个更大随机游走任务（19个状态取代5个状态）的原因是什么呢？在小任务上n的不同值得优势会改变吗？把左边得收益从0改到-1得原因呢？这会改变n的最优值吗？**

* 更小的步长会把优势转移到更小的n,因为当n &ge;(#states-1)/2(#states 是奇数)算法更新所有访问过的状态（通过最终奖励）。这意味着算法只对&alpha; 值大小进行更改，不储存或是引导。

* 在左边加上-1奖励有利于较小的n值，因为在较长的幕中，较大的n值将不得不通过终端奖励（现在是-1而不是0）更新许多状态从而增加方差。

### Exercise7.4

**证明Sarsa算法的n步回报（式7.4）可以被严格写成一个新的时序差分的形式，如下所示**
$$
\begin{align}
\end{align}
$$

$$
\begin{align}
&G_{t:t+n}=Q_{t-1}(S_{t},A_t) +\sum_{k=t}^{min(t+n,T)-1}\gamma^{k-t}[R_{k+1}+\gamma Q_k{(S_{k+1},A_{k_1})-Q_{k-1}(S_{k},A_k)}]\tag{7.6}\\
&G_{t:t+n}=R_{t+1}+\gamma R_{t+2}+\dots+\gamma^{n-1}R_{t+n}+\gamma^{n}Q_{t+n-1}\tag{7.4}
\end{align}
$$

$$
\begin{align}
&当n\ge1,0\le1\le T-n,G_{t:t+n}=G_{t}(如果t+n>T)\\
&设\tau=min(t+n,T)-1\\
&\sum_{k=t}^{\tau}\gamma^{k-t}[R_{t+1}+\gamma Q_{k}(S_{k+1},A_{k+1})-Q_{k-1}(S_k,A_k)]\\
&=\sum_{k=t}^\tau\gamma^{k-t}R_{t+1}+\gamma\sum_{k=t}^{\tau}\gamma^{k-t}Q_{k}(S_{k+1},A_{k+1}-\sum_{k=t}^{\tau}\gamma^{k-t}Q_{k-1}(S_{k},A_{k}))\\
&=G_{t:t+n}-\prod{\{t+n<T\}}\gamma^nQ_{t+n-1}(S_{t+n},A_{t+n})+\gamma^{\tau}Q_{\tau}(S_{\tau},A_{\tau})-Q_{t-1}(S_{t},A_{t})\\
&=G_{t:t+n}-Q_{t-1}(S_{t},A_{t})
\end{align}
$$

### Exercise 7.6

**证明上面式子中的控制变量不会改变回报的期望值**
$$
G_{t:h}=\rho_{t}(R_{t+1}+\gamma G_{t+1:h}+(1-\rho_{t})V_{h-1}(S_[t])),t<h<\rho \\ \tag{7.13}
G_{t:h}=R_{t+1}+\gamma(\rho_{t+1}G_{t+1:h}+V_{h-1}(S_{t+1})-\rho_{t+1}Q_{h-1}(S_{t+1},A_{t+1}))\\
=R_{t+1}+\gamma\rho_{t+1}(G_{t+1:h}-Q_{h-1}(S_{t+1},A_{t+1}))+\gamma \bar{V}_{h-1}(S_{t+1}),t<h\le T
$$

$$
\begin{align}
在7.13中我们有
E[(1-\rho_{t})V_{h-1}(S_{t})]&=E_{b}[(1-\rho_{t})V_{h-1}(S_{t})]\\
&=E_{b}[(1-\rho_{t})]E_{b}[V_{h-1}(S_{t})]\\
&=0\\
7.14中我们有\\
&E_{b}[\bar{V}_{h-1}(S_{t+1})-\rho_{t+1}Q_{h-1}(S_{t+1},A_{t+1})|S_{t+1}]\\
&=\sum_{a}\pi(a|S_{t+1})Q_{h-1}(S_{t+1},a)-\sum_{a}b(a|S_{t+1})\frac{\pi(a|S_{t+1})}{b(a|S_{t+1})}Q_{h-1}(S_{t+1},a)\\
&=0

\end{align}
$$

### Exercise7.8

**证明如果近似状态价值函数不变，则通用（离轨策略）版本的n步回报（式7.13）仍然可以间接地写为基于状态地TD误差（式6.5）的和的形式**
$$
\begin{align}
&更新目标G_{t:h}=\rho(R_{t+1}+\gamma G_{t+1:h}+(1-\rho_{t}V_{h-1}(S_{t})))\\
&认定state-value功能不变然后TD误差是\\
&\delta_{t}=R_{t+1}+\gamma V(S_{t+1})-V(S_{t})\\
G_{t:h}-V(S_{t})&=\rho_{t}(R_{t+1}+\gamma G_{t+1:h}-V(S_{t}))\\
&=\rho_{t}(R_{t+1}+\gamma [G_{t+1:h}-V(S_{t+1})]+\gamma V(S_{t+1})-V(S_{t}))\\
&=\rho_{t}\delta_{t}+\rho_{t}\gamma[G_{t+1:h}-V(S_{t+1})]\\
&=\sum_{i=t}^{min(h,T)-1}\rho_{t:i}\gamma^{ i-t}\delta_{i}
\end{align}
$$

### Exercise7.9

**针对动作版本的离轨策略n步回报（式7.14）和期望SarsaTD误差（式7.9）重复以上习题**
$$
动作价值更新G_{t:h}=R_{t+1}+\gamma\rho_{t+1}(G_{t+1:h}-Q_{h-1}(S_{t+1},A_{t+1})-\gamma\bar{V}_{h-1}(S_h))\\
其中\bar{V}_{h}=\sum_{a}\pi(a|S_{h})Q(S_{h},a)\\
我们认为动作价值函数在迭代过程中不变，定义\\
\delta_{t}=R_{t+1}+\gamma\bar{V}(S_{t+1})-Q(S_{t},A_{t})\\
=\sum_{i=y}^{min(h,T)-1}\gamma^{i-t}\rho_{t+1:i}\delta_{i}
$$
