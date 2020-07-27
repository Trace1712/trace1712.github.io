```text
layout:     post
title:      Reinforcement Learning An Introduction 
subtitle:   第四章
date:       2020-07-27
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning
```

# <center>第四章</center>

### Exercise4.1

| 终点 | 1    | 2    | 3    |
| ---- | ---- | ---- | ---- |
| 4    | 5    | 6    | 7    |
| 8    | 9    | 10   | 11   |
| 12   | 13   | 14   | 终点 |

**在上面的网格图中，可以上下左右移动，每次转移R<sub>t</sub>=-1，如果在智能体移除表格收益为0，如果&pi;是等概率随机策略，那么q<sub>&pi;</sub>（11，down） 是多少？q<sub>&pi;></sub>(7,down)呢？**

q<sub>&pi;</sub>(11,down) = -1因为移动了到了最终状态，q<sub>&pi;</sub>(7,down)=-15(不知道为啥)

### Exercise4.3

**对于动作价值函数q<sub>&pi;</sub>以及其逼近序列函数q<sub>0</sub>,q<sub>1</sub>,q<sub>2</sub>...,类似于式子 (4.3),(4.4),(4.5)的公式是什么？**
$$
\begin{align}
&v_\pi(s)=E_\pi[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s)]\tag{4.3}\\
&v_\pi(s)=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)[r+\gamma v_{\pi}(s')]\tag{4.4}\\
&v_{\pi+1}(s)=\sum_{a}\pi(a|s)\sum_{s',r}p(s',r|s,a)[r +\gamma v_{k}(s')]\tag{4.5}
\end{align}
$$

$$
q_{k+1}(s,a)=\sum_{s',r}p(s',r|s,a)[r+\gamma\sum_{a'}\pi(a'|s)q_{k}(s',a')]
$$

### Exercise4.5

**如何为动作价值定义策略迭代？请用类似于这里给出的关于v<sub>\*</sub>的算法写出关于q<sub>\*</sub>的完整算法。**
$$
v_\pi{s}=\sum_{a\in\mathcal{A}(s)}\pi(a|s)q_{\pi}(s,a)\\
q_{\pi}(s,\pi^{’}(s))\ge\sum_{a\in\mathcal{A}(s)}\pi(a|s)q_{\pi}(s,a)\
$$

if  &pi;<sup>'</sup> is greedy with respet to &pi;.

### Exercise4.6

**假设只能考虑&varepsilon;-柔性策略，即在每个状态s中选择每个动作概率至少为&varepsilon;/|A(s)|。请定性描述在v<sub> \*</sub>的策略迭代算法中，步骤3，步骤2和步骤1所需的更改。**
$$
\begin{align}
&1.不变\\
&2.需要重新写v(s)\leftarrow\sum_{a\in\mathcal{A}}\pi(a|s)\sum_{s',r}p(s^{'},r|s,a)[r+\gamma V(s')]\\
&3.构造一个贪心的策略，在每一个的贪心的动作上面加权重，但是是\varepsilon-柔性策略
\end{align}
$$

### Exercise4.8

**为什么赌徒问题的最优策略是一个如此奇怪的形式？特别是，当赌资为50时，他会选择全部投注，而当赌资时51时却不是这样。为啥？**

因为硬币是对我们不利的，所以我们要尽量减少我们的流言数量。在50岁时，我们以0.4的概率获胜。在51，如果我们赌小，我们可以得到52，但如果我们输了，我们仍然只回到50，我们可以再次与概率0.4。

### Exercise4.10

与价值迭代更新公式（4.10）类似的动作价值函数q<sub>k+1</sub>(s,a)更新公式是什么？
$$
q_{k+1}=\max_{a'}\sum_{a'}p(s',r|s,a)[r +\gamma q_{k}(s',a')]
$$
