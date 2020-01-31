# Lecture 03 - Jan 14 2020

## Refer to powerpoint 3

Slide 21 - Little o is stronger than Big O

Slide 22 - If $f(n)=n^3$ and $g(n)=21n^2$ then $O(n^3+21n^2)=O(n^3)$

Slide 23 - Recall that if the conditions are met, $\lim\limits_{n\to\infty}\frac{f(n)}{g(n)}=\lim\limits_{n\to\infty}\frac{f'(n)}{g'(n)}$

Slide 24 - Proof

$$\lim_{n\to\infty}\frac{f(n)}{g(n)}=\lim_{n\to\infty}\left[\frac{c_dn^d}{n^d}+\frac{c_{d-1}n^{d-1}}{n^d}+\dots+\frac{c_0}{n^d}\right]=c_d$$
which is some constant

Slide 25 -
$n(2+\sin\frac{n\pi}{2})$ is bounded below and above by $n$ and $3n$ respectively.

$$n\leq n(2+\sin\frac{n\pi}{2})\leq 3n, \forall n\geq 1$$
Thus $n(2+\sin\frac{n\pi}{2})\in\Theta(n)$

Slide 27 - Compare $\log n$ and $n^d$ for any $d>0$. We will do the limit thing again.

$$\lim_{n\to\infty}\frac{\log n}{n^d}=\lim_{n\to\infty}\frac{1/n}{dn^{d-1}}=\lim_{n\to\infty}\frac{1}{dn^d}=0$$

So, $\log n\in o(n^d)$. Intuitively, we can say that $n^d$ dominates $\log n$.

Slide 42 - Arithmetic sequences. Let $a,d$ be constants.

$$s_1=\sum_{i=0}^\infty\frac{1}{2^i}=2$$

$$s_2=\sum_{i=1}^ni=1+2+3+\dots+n=\frac{n(n+1)}{2}$$

$$s_3=\sum_{i=0}^\infty \frac{i}{2^i}=\frac{0}{1}+\frac{1}{2}+\frac{2}{4}+\dots$$

Consider $2s_3=1+\frac{2}{2}+\frac{3}{2^2}+\dots$. Now consider $2s_3-s_1=s_1=2$

Slide 43 - Consider 
$$\log 2^n+n^22^{3\log n}$$
 which is equal to, by the log laws,
 $$n\log 2+n^22^{\log n^3}=n+n^2(n^3)^{\log 2}=n+n^5$$

 Slide 30 - The basic approach is to work from inside to outside.
 - The body of the loop is $\Theta(1)$ time.
 - \# of iterations: $\sum\limits_{i=1}^n\sum\limits_{j=i}^n1=\sum\limits_{i=1}^n(n-i+1)=\sum\limits_{i=1}^ni=\frac{n(n+1)}{2}\in\Theta(n^2)$

We get $\Theta$ since we maintained equality the entire time.

Sloppy method: 
$$\sum_{i=1}^n(n-i+1)\leq \sum_{i=1}^nn=n\cdot n=n^2\in O(n)$$
We still must prove $\Omega(n^2)$.
$$\sum_{i=1}^n(n-i+1)= \sum_{i=1}^ni\geq \sum_{i=n/2}^ni\geq\sum_{i=n/2}^n\frac{n}{2}=\frac{n^2}{4}$$

Slide 31
Direct method

\# of iterations: 
$$\sum_{i=1}^n\sum_{j=i}^n(j-i+1)=\sum_{i=1}^n\left[\sum_{j=i}^nj-\sum_{j=i}^n(i-1)\right]$$
$$=\sum_{i=1}^n\left[\sum_{j=i}^nj-(n-i+1)(i-1)\right]$$
$$=\sum_{i=1}^n\left[\sum_{j=1}^nj-\sum_{j=1}^{i-1}j-(n-i+1)(i-1)\right]$$
$$=\sum_{i=1}^n\left[\frac{n(n+1)}{2}-\frac{(i-1)(i)}{2}-(n-i+1)(i-1)\right]$$
$$=\sum_{i=1}^n\left[\frac{1}{2}i^2+\left(-n-\frac{3}{2}\right)i+\frac{n(n-1)}{2}+n+1\right]$$
$$\frac{1}{2}\sum_{i=1}^ni^2+(-n-3/2)\sum_{i=1}^ni+n\left(\frac{n(n-1)}{2}+n+1\right)$$
$$=\dots$$
$$\frac{1}{6}n^3+\frac{1}{2}n^2+\frac{1}{3}n\in\Theta(n^3)$$

Easy to get a big O bound:

\# of iterations: 
$$\sum_{i=1}^n\sum_{j=i}^n(j-i+1)\leq \sum\sum n=n^3$$
$$\implies O(n^3)$$

Lower bound:
$$\sum_{i=1}^n\sum_{j=i}^n\sum_{k=i}^j 1$$
$$\geq \sum_{i=1}^{n/3}\sum_{j=i}^n\sum_{k=i}^j 1$$
$$\geq\sum_{i=1}^{n/3}\sum_{j=2n/3}^n\sum_{k=i}^j 1\qquad\text{since }i\leq \frac{n}{3}$$
$$\sum_{i=1}^{n/3}\sum_{j=2n/3}^n\sum_{k=n/3}^{2n/3}1\qquad\text{since }j\geq \frac{2n}{3},i\leq\frac{n}{3}$$
$$\geq \sum_{i=1}^{n/3}\sum_{j=2n/3}^n\frac{n}{3}$$
$$\geq \sum_{i=1}^{n/3}\frac{n}{3}\cdot\frac{n}{3}$$
$$\geq \frac{n}{3}\cdot\frac{n}{3}\cdot\frac{n}{3}=\frac{n^3}{27}\in\Omega(n^3)$$