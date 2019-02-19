# $SSVD$算法

$\textbf{孙傲 厦门大学经济学院}$

$SSVD$(sparse singular value decomposition)是一种在$SVD$分解基础上进一步对得到的对$SVD$ layer 稀疏化，即，让 $\vec{u}$ 和 $\vec{v}$ 向量稀疏化的方法。

## Recast $SVD$

let $X = (x_{ij}) \in \mathbb{R}^{p*n}​$ be the matrix with indices $i = 1,2,...,p​$ and $j = 1, ..., n​$. $SVD​$ of $X​$ can be written as:
$$
X \approx USV^T = \sum_{k=1}^r s_ku_kv_k^t \tag{1}
$$
the $SVD$ is the sum of rank one matrices called $SVD$ layer.

## $SSVD$ Algorithm

首先，将矩阵 $X$ 做 $SVD$ 分解，得到第一层$svd$ layer写为 $svd$ triplet $(d_1, u_1, v_1)$ , 为了方便，简写为 $(d,u,v)$ 然后最小化
$$
\begin{align}
min \quad \quad &||X -suv^t||^2_F + \lambda_uP_1(su) + \lambda_vP(sv) \tag{2} \\
\\
where \quad &P_1(su) = s\sum_{i=1}^n w_{1,i}u_i \tag{3}\\
&P_2(sv) = s\sum_{j=1}^p w_{2,j}u_i \tag{4}
\end{align}
$$

### 确定权重$w_{1,i}$ 和 $w_{2,j}$

固定 $u$ ，可将（2）式改写成为
$$
min \quad ||Y - (I_p\bigotimes u)\tilde{v}||^2 + \lambda_vP_2(\tilde{v}) \tag{5}
$$
可将（5）式的前半部分看出一个一元线性回归， 已知线性回归 $Y = AX$ 的 $\hat{Y} = (A^tA)^{-1}A^tY$  ，这里 $Y$ 是因变量， $\tilde{v}$ 是自变量，则$\tilde{v}$ 的估计值可写成
$$
\begin{align}
\hat{\tilde{v}} &= [(I_p\bigotimes u)^tI_p\bigotimes u]^{-1}(I_p \bigotimes u)^tY \\
&=X^tu \tag{6}
\end{align}
$$

令 $w_{2,j} = |\hat{\tilde{v}}|^{-\gamma_2}​$ 

同理，固定 $v​$ ，用同样的方法得到权重 $$w_{1,i} = |\hat{\tilde{u}}|^{-\gamma_1}​$$   
$$
\hat{\tilde{u}} = Xv \tag{7}
$$

### 估计 $\tilde{u}$ 和 $\tilde{v}$ 

这是 $ssvd​$ 的核心步骤，这里的估计 $\tilde{u}, \tilde{v}​$ 是将上文得到的 $svd​$ triplet 中的 $u, v​$ 稀疏化。
$$
min_u \quad||X-u\tilde{v}^t||^2_F + \lambda_v\sum^d_{j=1}w_{2,j}|\tilde{v}_j| \tag{8}
$$

这个式子可理解为：如果没有后面的 $l1$ penalty 那么 最小值就是 $svd$ 中的 $v$ ，而加了 $l1$ penalty后，$\tilde{v}$ 相对于 $v$ ，会有更多的 entries shrink to 0.

为了求 $\tilde{v}$ ，首先将式（8）化简成:
$$
min_{\tilde{v}} \quad ||x||_F^2+\lambda_v\sum_{j=1}^p[\tilde{v}^2_j-2\tilde{v}(X^tu)_j+\lambda_vw_{2,j}|\tilde{v}_j|]\tag{9}
$$
$lemma 1$
$$
min_\beta \quad \beta^2-2y\beta+2\lambda|\beta| \\
then \quad \hat{\beta} = sign(y)(|y|-\lambda)_+
$$
根据 $lemma1$ ，求得 
$$
\hat{v}_j = sign((X^tu)_j)[|(X^tu)_j|-\frac{\lambda_vw_{2,j}}{2}]_+ \tag{10}
$$
同理，可求得 $\tilde{u}_i$ 的估计值
$$
\hat{u}_i = sign((Xv)_i)[|(Xv)_i|-\frac{\lambda_vw_{1,i}}{2}]_+ \tag{11}
$$

### 选择合适的 $\lambda_u$ 和 $\lambda_v$ 

$\lambda_u$ 和 $\lambda_v$ 是两个 tuning parameters, 代表着是应该让 $\tilde{u}\tilde{v}^t$ 接近 $X$ 还是让这两个向量的元素更稀疏。 在选择tuning parameter 的方法上，出现了 $ssvd$ 和 $s4vd$ 两种不用的方法。其中 $ssvd$ 是用 $BIC$ 来作为选择tuning parameter 的标准。

$def1$ : degree of spasity (df) 为 $\tilde{u}$ 和 $\tilde{v}$ 中的0元素的个数。

由式（10）和（11）可以看出，

 $corollary1$
$$
\tilde{u}_i=0 \quad if \quad |(Xv)_i|<\frac{\lambda_uw_{1,i}}{2} \\
\tilde{v}_j=0 \quad if \quad |(X^tu)_j|<\frac{\lambda_vw_{2,j}}{2}
$$
$def2$ : BIC
$$
BIC(\lambda_v) = \frac{||Y-\hat{Y}||^2}{np\hat{\sigma}^2}+\frac{log(np)}{np}\hat{df}(\lambda_v)\tag{12}
$$
where $\hat{\sigma}^2 = \frac{\sum e_i^2}{n-1}$ 即（6）式回归估计的误差，$\hat{Y} = \tilde{u}\tilde{v}^t$ 。

那么
$$
\lambda_v=argmin_{\lambda_v}BIC(\lambda_v)\tag{13}
$$
同理
$$
\lambda_u=argmin_{\lambda_u}BIC(\lambda_u)\tag{13}
$$

## 总结

$ssvd$算法可归结为：

step1 用 $svd$ 求得 $X$ 的第一组 triplet $(s,u,v)$

step2 求 $\hat{u}$ 和 $\hat{v}$ 
$$
\begin{align}
&\hat{u}_i = sign((Xv)_i)[|(Xv)_i|-\frac{\lambda_vw_{1,i}}{2}]_+  \\
&\lambda_u=argmin_{\lambda_u}BIC(\lambda_u) \\
&\hat{v}_j = sign((X^tu)_j)[|(X^tu)_j|-\frac{\lambda_vw_{2,j}}{2}]_+ \\
&\lambda_v=argmin_{\lambda_v}BIC(\lambda_v)
\end{align}
$$
step3 update
$$
u_{new} = \hat{u}*\frac{1}{||\hat{u}||}\\
v_{new} = \hat{v}*\frac{1}{||\hat{v}||}\\

v = v_{old} \quad u = u_{old}
$$
until convergence.

step4 收敛后，得收敛时得 $u_{new}​$ 和 $v_{new}​$ ，那么 $s = u^txv​$, 到这里，算得第一层 $ssvd​$ layer，set $X = X - suv^t​$ 再次进入循环。