# PAR->WPAR

$\Longrightarrow$

由划分问题的一个实例$A$,构造广义划分问题的一个实例$B$

$B = A \cup \{b_1\} \cup \{b_2\}$

记$$S_A = \sum\limits_{a_i\in{A}}S(a_i)$$

我们令
$$
\begin{aligned}
&S\left(b_1\right) = [ck + (c-1)0.5]S_A \\
&S\left(b_2\right) = kS_A\qquad\qquad\qquad\qquad(k \geqq 1)
\end{aligned}
$$
显然$S(b_1) + S(b_2) > cS_A$

所以$b1,b2$必在广义划分的两侧

设$A^{''} \subset A$  其中的元素与b1在广义划分的同侧

则广义划分两侧的权重之和分别为  $S(b1) + \sum\limits_{a_i\in{A}}S(a_i)$ 和 $S(b2) + \sum\limits_{a_i\in{A-A^{''}}}S(a_i)$

设 $x = \sum\limits_{a_i\in{A}}S(a_i)$

对于等式 $S(b_1) + x = c(S(b_2) + S_A - x)$
$$
\begin{align}
&\Leftrightarrow[ck+(c-1)0.5]S_A + x = ckS_A + cS_A - cx \\
&\Leftrightarrow(1+c)x = \frac{1+c}{2}S_A \\
&\Leftrightarrow x = \frac{1}{2}S_A \tag{1}\label{Res}
\end{align}
$$
 所以原划分问题的实例回答yes,则构造的广义划分实例回答yes

反之构造的广义划分实例回答no

$\Longleftarrow$

对于广义划分的这个实例 $B = A \cup \{b_1\} \cup \{b_2\}$

 相应的划分实例为$A$

根据前面推导，若存在广义划分，则$b_1$和$b_2$必然位于不同划分

由$\eqref{Res}$知，若广义划分回答yes,则满足$\exist \ A^{''} \sub A$

$\sum\limits_{a_i\in{A{''}}}S(a_i) = \frac{1}{2}\sum\limits_{a_i\in{A}}S(a_i)$

所以划分问题回答yes,反之原划分问题回答no

综上 规约的时间复杂度是多项式的，所以PAR归约WPAR成立。







