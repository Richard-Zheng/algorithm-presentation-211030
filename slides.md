---
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# 问题分析与线性优化策略

GDUT-//undefined

---

# 目标

- 写信息学竞赛题的一般流程
- 如何高效训练？
- 优化——面对题目时最重要的思维方向
- 简单的数学推导技巧

## 关于例题

课后建议完成课上例题。我认为有训练价值的题，都会写题号，题目号格式与洛谷相同。

---
layout: center
---

# Part 1: 问题分析

---

# 竞赛题目的构成

- 题目描述
- 数据范围

---

## 题目描述

![image.png](https://i.loli.net/2021/10/22/tg13PR8YhbdvJFr.png)

- 背景：一般可以忽略
- 题目内容：问题是什么

**题目描述一定要和数据范围一起看**

---

# 例题

## 斐波那契数列

将斐波那契数列的第$n$项记为$a_n$. 输出$a_n\bmod 10^7$

<v-click>

数据范围：？

</v-click>

---

# 取模运算（$\bmod$）

我们可以把取模运算想象成将数轴弯曲绕成一个圆圈。

<div class="absolute right-30px top-30px">
<img src="https://nrich.maths.org/content/id/4350/Clock2.gif" />
</div>

参考右图中的时钟，$13\bmod 12=1$. 取模运算就是求相除后得到的余数。

## 运算法则

与四则运算类似，但是除法例外。其规则如下：

- $(a+b)\bmod p=(a\bmod p+b\bmod p)\bmod p$
- $(a-b)\bmod p=(a\bmod p-b\bmod p)\bmod p$
- $(a\times b)\bmod p=(a\bmod p \times b\bmod p)\bmod p$
- $a^{b}\bmod p=((a\bmod p)^b)\bmod p$

---

## P1720
 
将斐波那契数列的第$n$项记为$a_n$. 输出$a_n\bmod 10^7$

数据范围$1\leq t\leq 10, 1\leq n\leq 1000$

<v-click>

解法：模拟，总复杂度$Tn\approx 10^4$

```cpp
#include <iostream>
using namespace std;
const int MAXN = 1001;
const int MOD = 1e7;
int main() {
    int f[MAXN] = {0};
    f[1] = 1;
    f[2] = 1;
    for (int i = 3; i <= MAXN; i++) {
        f[i] = (f[i - 1] + f[i - 2]) % MOD;
    }
    int t, n; cin>>t;
    while(t--){
        cin>>n;
        cout<<f[n]<<endl;
    }
}
```

</v-click>

---

## P1962

数据范围$T=1, 1\leq n\leq 10^{18}$

<v-click>

解法：直接模拟会超时，需要使用**矩阵快速幂**将单次求值复杂度优化至$\log_2n$，总复杂度$Tn\approx 60$，轻松AC！

![dot-product](https://www.shuxuele.com/algebra/images/matrix-multiply-a.svg)

“点积”是把**对称的元素相乘**，然后把结果加起来：

(1, 2, 3) • (7, 9, 11) = 1×7 + 2×9 + 3×11 = 58

$$
\begin{bmatrix} fib\left( i\right) \\ fib\left( i-1\right) \end{bmatrix}=\begin{bmatrix} fib\left( i-1\right) \\ fib\left( i-2\right) \end{bmatrix}\begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}
$$
$$
\begin{bmatrix} f\left( n\right) \\ f\left( n-1\right) \end{bmatrix}=\begin{bmatrix} f\left( 2\right) \\ f\left( 1\right) \end{bmatrix}\begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}^{n-2}
$$

</v-click>

---

## P5110

数据范围$1\leq T\leq 5\times 10^7, 1\leq n\leq 10^9$

<v-click>

解法：$Tlog_2n \approx 1.5\times 10^9 > 10^8$，可以看出，前一个优化也过不了，需要通过更进一步的技巧，我们称之为**光速幂**。

至此已然达到**紫题/金牌题**难度，故在这里就不展开讲了。

</v-click>

---

# 时间复杂度与算法对照表

由时间复杂度推测可能的算法。在不同数据范围下，代码的时间复杂度和算法该如何选择？

<div class="absolute right-30px top-15px">
参考：<a href="https://www.acwing.com/blog/content/32/">由数据范围反推算法复杂度以及算法内容, yxc, AcWing</a>
</div>

- $n \le 30$, 指数级别, dfs+剪枝，状态压缩dp
- $n \le 100$ => $O(n^3)$，floyd，dp，高斯消元
- $n \le 1000$ => $O(n^2)$，$O(n^2logn)$，dp，二分，朴素版Dijkstra、朴素版Prim、Bellman-Ford
- $n \le 10000$ => $O(n * \sqrt n)$，块状链表、分块、莫队
- $n \le 100000$ => $O(nlogn)$ => 各种sort，线段树、树状数组、set/map、heap、拓扑排序、dijkstra+heap、prim+heap、spfa、求凸包、求半平面交、二分、CDQ分治、整体二分
- $n \le 1000000$ => $O(n)$, 以及常数较小的 $O(nlogn)$ 算法 => 单调队列、 hash、双指针扫描、并查集，kmp、AC自动机，常数比较小的 $O(nlogn)$ 的做法：sort、树状数组、heap、dijkstra、spfa
- $n \le 10000000$ => $O(n)$，双指针扫描、kmp、AC自动机、线性筛素数
- $n \le 10^9$ => $O(\sqrt n)$，判断质数
- $n \le 10^{18}$ => $O(logn)$，最大公约数，快速幂
- $n \le 10^{1000}$ => $O((logn)^2)$，高精度加减乘除
- $n \le 10^{100000}$ => $O(logk \times loglogk)$（k表示位数），高精度加减、FFT/NTT

小技巧

$\log_2 10^n =3n$
64MB至多开1600万个int


C++一般$1$s能计算$10^7$~$10^8$次，在这种情况下，C++代码中的操作次数控制在 $10^7$为最佳