---
layout:     post
title:      Reinforcement Learning An Introduction
subtitle:   第六章
date:       2020-08-03
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning
---


# Reinforcement Learning An Introduction

## <center>第六章</center>

### Exercise6.1

**如果V在幕中发生了变化，那么式（6.6）只能近似成立；等式两侧差在哪里？令V<sub>t</sub>表示在t时刻TD误差公式（6.5）和TD更新公式（6.2）中使用的状态价值的数组。重新进行上面的推导，推出 为了让等式右侧扔等于左侧的蒙特卡洛误差，需要在TD误差之和上加上额外项。**
$$
\begin{align}
&V(S_t)\leftarrow V(S_t) + \alpha[R_{t+1}+\gamma V(S_{t+1})-V(S_{t})] \tag{6.2}\\
&\delta=R_{t+1}+\gamma V(S_{t+1})-V(S_{t}) \tag{6.5}\\
&G_{t}-\gamma V(S_{t}) \\&= R_{t+1} + \gamma G_{t+1} -V(S_{t})+\gamma V(S_{t+1})-\gamma V(S_{t+1})\\
&=\delta_{t}+\gamma(G_{t+1}-V_{t+1})\\
&=\delta_{t}+\gamma\delta_{t+1}+\gamma^2(G_{t+2}-V(S_{t+1}))\\
&=\delta_{t}+\gamma\delta_{t+1}+\gamma^2\delta_{t+2}+\dots+\gamma^{T-t-1}\delta_{T-1}+\gamma^{T-t}(G_{T}-V(S_{T}))\\
&=\delta_{t}+\gamma\delta_{t+1}+\gamma^{2}\delta_{t+2}+\dots+\gamma^{T-t-1}\delta_{T-1}+\gamma^{T-t}(0-0)\\
&=\sum_{k=t}^{T-1}\gamma^{k-t}\delta_{k}\tag{6.6}
\end{align}
$$

$$
\begin{align}
&\delta_{t}=R_{t+1}+\gamma V_{t}(S_{t+1})-V_{t}(S_{t})\\
G_{t}-V(S_{t})&=R_{t+1}+\gamma G_{t+1}-V_{t}(S_{t})\\
&=\delta_{t}+\gamma[G_{t+1}-V_{t+1}(S_{t+1})] + \gamma[V_{t+1}(S_{t+1})-V_{t}(S_{t+1})]\\
&=\delta_{t}+\gamma[G_{t+1}-V_{t+1}(S_{t+1})] + \alpha\gamma[R_{t+2}+\gamma V_{t}S(t+2)-V_{t}(S_{t+1})]\\
&=\sum_{k=t}^{T-1}\gamma^{k-t}\delta_{k}+\alpha\sum_{k=t}^{T-2}\gamma^{k-t+1}[R_{k+2}+\gamma V_{k}(S_{k+2})-V_{k}(S_{K+1})]
\end{align}
$$

### Exercise6.3

**在随机游走例子的左图中，第一幕只导致V(A)发生了变化。由此，你觉得第一幕发生了什么？为什么只有这一个状态的估值改变了？它的值到底改变了多少？**

除终端状态外，所有状态都初始化为相同的值（终端状态初始必为0），且非终端转换的奖励为0，因此TD(0)更新对第一次传递无法直接导致终止的状态不执行任何操作。
$$
\begin{align}
v_{1}(A)&=V_{0}(A) +\alpha[0+\gamma*0+V_{0}(A)]\\
&=(1-\alpha)V_{0}(A)\\
&=0.9 * 0.5
\end{align}
$$

### Exercise6.4

**在随机游走例子的右图中，具体结果与步长参数&alpha;的值是相关的。如果一个更宽广的值域中取值，会影响哪种算法更好的结论吗？是否存在另一个固定的&alpha;，使得两种算法都能表现除比图中更好的性能？**

* 前面给出的有关TD益处的一般论点与&alpha;无关
* &alpha;的增加使得曲线更加弯曲
* &alpha;的减少使得曲线更加平滑，但收敛速度较缓
* 我们在这看到足够的范围来决定两种方法之间关系

### Exercise6.5

**在随机游走例子中，TD方法的均方根误差先下降后上升，尤其在&alpha;较大的时候更加明显。为什么会这样？这种情况总是会发生还是与近似价值函数如何初始化相关。**

状态C恰好被初始化为真实值。随着训练开始，外部状态会发生更新(使各个状态更精确)，从而减少所有状态的错误。这会一直发生到外部状态的剩余误差不确定性传播到C为止。较高的&alpha;值使得此效应更加明显，因此这些情况下的值估计更容易改变。（不知道啥意思）

### Exercise6.6

**在例子6.2中的随机游走任务中，状态A~E的真实价格分别是：1/6，2/6，3/6，4/6和5/6。描述至少两种不同的计算这些值方法。猜猜实际中我们使用的是哪种方法？为什么？**

也要认识到对称现在意味着
$$
\begin{align}
&V(C)=0.5\\
&V(E)=1/2*1+1/2*V(D)\\
&=1/2+1/4[V(C)+V(E)]\\
所以&V(E)=5/6，然后可以计算出V(D)=4/6然后以可以计算其他状态
\end{align}
$$

### Exercise6.7

**设计一个离轨策略版的TD(0)更新算法，使其可以用于任意的目标策略&pi;，且公式包含行动策略b<sub></sub>0</sub>.注意：在每个时刻t，要使用重要度采样比&rho;<sub>t:t</sub>(式5.3)**
$$
\begin{align}
v_{\pi}(s)&=E[\rho_{t:T-1}G_{t}|S_{t}=s]\\
&=E[\rho_{t:T-1}R_{t+1}+\gamma\rho_{t+1:T-1}G_{t+1}|S_{t}=s]\\
&=\rho_{t:t}E[R_{t+1}|S_{t}=s]+\gamma\rho_{t:t}E[\rho_{t+1:T-1}G_{t+1}|S_{t}=s]\\
&=\rho_{t:t}(E[R_{t+1}|S_{t}=s]+E[\rho_{t+1:T-1}G_{t}|S_{t}=s])\\
&=\rho_{t:t}(r(s,A_{t})+v_{\pi}(S_{t+1}))\\
离轨策略TD(0)更新就是&V(S_{t})\leftarrow V(S_{t})+\alpha[\rho_{t:t}R_{t+1}+\rho_{t:t}\gamma V(S_{t+1})-V(S_{t})]
\end{align}
$$

### Exercise6.8

**证明公式6.6的状态-动作二元组版本也是称里的，“状态-动作”二元组的TD误差是**
$$
\delta_{t}=R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_{t},A_{t})
$$
**这里假设价值函数在不同时刻不发生改变**
$$
\begin{align}
&\delta_{t}=R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_{t},A_{t})\\
&蒙特卡洛误差可以写为\\
&G_{t}-Q(S_{t},A_{t})\\
&=R_{t+1}+\gamma G_{t+1}-Q(S_{t},A_{t})\\
&=\delta_{t}-\gamma[Q(S_{t+1},A_{t+1})+G_{t+1}]\\
&=\sum_{k=t}^{T-1}\gamma^{k-t}\delta_{k}
\end{align}
$$

### Exercise6.11

**为什么Q学习被认为是一种离轨策略**

对收益进行抽样，就好像代理对Q遵守 贪婪策略。

### Exercise6.13

**使用&epsilon;-贪心目标策略的双期望Sarsa的更新步骤是怎样的？**
$$
SARSA有更新\\
Q(S_{t},A_{t})\leftarrow Q(S_{t},A_{t})+\alpha[R_{t+1}+\gamma E[Q(S_{t+1})|S_{t+1}]-Q(S_{t},A_{t})]\\
更新S_{t},A_{t}\\
R_{t+1}+\gamma\sum_{a}\pi(a|S_{t+1})Q(S_{t+1},a)-Q(S_{t},A_{t})\\
双重期望的SARSA将保留两个Q数组，并以相同的概率选择每个时间步更新其中一个\\
通过一个\epsilon策略，我们会增加Q_1(S_{t},A_{t})通过\\
\alpha[R_{t+1}+\gamma(\frac{\epsilon}{|\mathcal{A}(a)|}\sum_{a}Q_{2}(S_{t+1},a)+(1-\epsilon)\max_{a}\{Q_{2}(S_{t+1},a)\})-Q_1(S_t,A_t)]
$$
