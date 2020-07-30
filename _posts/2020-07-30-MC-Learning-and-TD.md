---
layout:     post
title:      MC-Learning and TD
subtitle:   summary
date:       2020-07-30
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning
---

# MDP(有限马尔可夫决策过程)

MDP是序列决策的经典形式化表达，通过<S,A,P,R,&gamma;>来计算价值函数，有著名的Bellman方程
$$
\begin{align}
&V_\pi(s)=\sum_{a\in\mathcal{A}}\pi(a|s)\sum_{s',r}p(s',r|(s,a))[r+\gamma V_\pi(s')]\\
&q_{\pi}(s,a)=\sum_{s',r}p(s',r|s,a)[r+\gamma\sum_{a'}\pi(a'|s')q_{\pi}(s',a')]
\end{align}
$$
MDP的缺点：需要知道<S,A,P,R,&gamma;>

# DP(动态规划)

DP是用来优化贝尔曼方程的，主要通过两种方法，策略迭代和价值迭代，其中策略迭代又分为策略评估和策略改进。这里不再细说，只是一个引入。

# MC Learning(蒙特卡洛学习)

* **蒙特卡罗方法又叫做统计模拟方法，它使用随机数(或伪随机数)来解决计算问题。**

* **为什么使用蒙特卡洛方法 ？**

  前面使用的MDP，需要通过五元组来计算价值函数，但我们很多情况下不能知道五个量，(比如p)那么就无法使用贝尔曼方程去求最优解。

* **蒙特卡洛方法如何解决？**

  考虑如何在给定一个策略的情况下，用蒙特卡洛算法来学习其状态价值函数。我们已经知道了一个状态的价值是从该状态开始的期望回报，即未来的折扣收益累计值得期望。当我们对所有经过这个状态之后产生得回报进行平均。**随着越来越多的回报被观察到，平均值就会收敛于期望**。

* **蒙特卡洛预测步骤**

  1. 在一个事件中第一次访问状态s的步骤t
  2. 增加计数N(s)&leftarrow;N(s)+1
  3. 增加整个返回值S S(s)&leftarrow;S(s)+G(t)
  4. 通过平均值V(s)=S(s)/N(s)评估价值函数
  5. V(s)&rightarrow;V<sub>&pi;</sub>(s) 当N(s)&rightarrow;无穷大

* 对于episode(幕)里的每一个状态S<sub>t</sub>，有一个收获G<sub>t</sub>，每碰到一次S<sub>t</sub>，使用下面的式子计算状态的平均价值V(s):
  $$
  \begin{align}
  &N(S_t)\leftarrow N(S_t)+1\\
  &V(S_t)\leftarrow+\frac{1}{N(S_t)}(G_t-V(S_t))
  \end{align}
  $$
  
* **蒙特卡洛学习的缺点**

  需要到episode尾才能更新V(s)

  

# TD(时序差分学习)

* TD learning中价值函数的更新方法

  TD Learning中才会把G<sub>t</sub>写成递归的形式。这样，每走一步都可以更新一次V。算法在估计某一个状态的价值时，用的是离开该状态时的即时奖励R<sub>t+1</sub>和下一个状态S<sub>t+1</sub>的预估状态价值乘以折扣因子&gamma;组成。

  
  $$
  \begin{align}
  &将值V_{t}更新为估计收益 R_{t+1} + \gamma V(S_{t+1}))\\
  &V(S_t)\leftarrow V(S_t)+\alpha\color {red}{(R_{t+1} + a)}\\
  &R_{t+1}+\gamma V(s_{t+1})&为TD 目标\\
  &\delta=R_{t+1}+\gamma V(S_{t+1}) -V(S_{t})&为TD误差
  \end{align}
  $$

# 比较

| 特性                                                         | DP   | MC   | TD   |
| ------------------------------------------------------------ | ---- | ---- | ---- |
| 是否需要完备的环境模型（需要知道 p(s′,r∣(s,a))p(s',r \vert (s,a))*p*(*s*′,*r*∣(*s*,*a*)) ） | Y    | N    | N    |
| 采样更新（计算基于采样得到的单个后继节点的样本数据）         | N    | Y    | Y    |
| 无需等待交互的最终结果                                       | Y    | N    | Y    |
| 根据幕来更新（MC到幕尾知道G<sub>t</sub>，才能开始对V(s)更新） | N    | Y    | N    |
| 基于已存在的VV*V*对VπV_\pi*V**π*估计                         | Y    | N    | Y    |

# 致谢

感谢这两个博主的文章

https://blog.csdn.net/liweibin1994/article/details/79111536/

https://blog.csdn.net/weixin_42815609/article/details/104034967

