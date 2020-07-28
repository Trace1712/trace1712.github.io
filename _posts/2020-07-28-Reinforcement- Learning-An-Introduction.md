---
layout:     post
title:      Reinforcement Learning An Introduction
subtitle:   第五章
date:       2020-07-28
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning
---


# Reinforcement  Learning An Introduction

# <center>第五章</center>

### Exercise5.6

**给定用b生成的回报，类似公式（5.6）用动作价值函数Q(s,a)代替状态价值函数V(s)，得到的式子是什么？**
$$
q_\pi(s,a)=E_{\pi}[\rho_{t+1}:T_{t-1}G_{T}|S_{t}=S,A_{T}=s]
$$
产生的回报来自b(行动策略)我们估计这个Q为
$$
Q(s,a)=\frac{\sum_{t\in \tau(s,a)}\rho_{t+1:T-1}G_t}{\sum_{t\in \tau(s,a)}\rho_{t+1:T-1}}
$$

### Exercise5.7

**实际使用普通重要度采样时，与图5.3保持一致，错误率一般都是下降的，但是对于加权重要度采样，错误率会随着训练一般都是下降的。但是对于加权重要度采样，错误率会先上升然后下降。为什么？**

当幕很少时，重要性采样比率很大可能性会变成0，因为行为策略将坚持小于20（因为他是随机的）。0碰巧接近于V<sub>&pi;</sub>。这种影响随着情节轨道多样化而减弱。<font color='red'>（不是很理解离轨策略估计）</font>

### Exercise5.8

**在图5.4和例子5.5中的结果中采用首次访问MC方法。假设在同样的问题中采用了每次访问型MC方法，估计器的方差会是无穷的吗？为什么**

会得，总和中所有都大于0，而且只会有更多项

### Exercise5.9

**对首次使用访问型蒙特卡洛的策略评估(5.1节)过程进行修改，要求采用2.4节中描述的对样本平均的增长式实现。**

不需要返回 s lists

移除最后两行，改为
$$
V(S_{t})\leftarrow V(S_{t})+ \frac{1}{T-t}[G_t-V(S_{t})]
$$

### Exercise5.10

$$
\begin{align}
&V_n=\frac{\sum_{k=1}^{n-1}W_kG_k}{\sum_{k=1}^{n-1}W_k},n\ge2\tag{5.7}\\
&V_{n+1}=V_n+\frac{W_n}{C_n}[G_n-V_n],n\ge1\tag{5.8}\\
&C_{n+1}=C_n+W_{n+1}
\end{align}
$$



**如何从式子5.7化为5.8**
$$
\begin{align}
V_n+\frac{W_n}{C_n}[G_n-V_n]=&(1-\frac{W_n}{C_n})V_n+\frac{W_n}{C_n}G_n\\
=&\frac{C_n-W_n}{C_n}\frac{\sum_{k+1}^{n-1}W_kG_k}{\sum_k^{n=1}W_k}+\frac{W_nG_n}{C_n}\\
=&\frac{C_{n-1}}{C_n} \frac{\sum_{k=1}^{n=1}W_kG_k}{C_{n-1}}\\
=&\frac{\sum_{k-1}^{n}W_kG_k}{C_n}
\end{align}
$$

### Exercise5.11

**在算法框内的离轨策略MC控制算法中，也许觉得W的更新应该采取重要度采样比**&pi;(A<sub>&pi;</sub>|S<sub>t</sub>)/b(A<sub>t</sub>|S<sub>t</sub>),**但它实际上却用**1/b(A<sub>t</sub>|S<sub>t</sub>)。**为什么是正确的？**
$$
\pi是贪婪的，因此\pi(a|s)=\prod{\{a=argmaxQ(s,a')\}}
$$
<font color="red">不是很懂</font>