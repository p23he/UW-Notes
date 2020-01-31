# CS240 - Lecture 7, Jan 28

## Last Class: Quicksort
- Ideally want to pivot to fall in the middle
  - 2 subproblems of size $n/2$ or $\lfloor\frac{n-1}{2}\rfloor$ and $\lfloor\frac{n+1}{2}\rfloor$
- The running time is $T(n)=T(n-1)+cn$, so the worst case running time is $O(n^2)$
  - Not likely to happen though
- On best case, our recurrence relation is $T(n)=T(\frac{n}{2})+cn$
  - Each level in recursion tree is $cn$ work, so we only need to find out how many levels there are now -> that is $\log n$!
  - Height: $\Theta(\log n)$, work per level: $cn$, base cases: $d$ some constant
  - Thus the running time is: $cn\cdot\log n+nd\in \Theta(n\log n)$
- Average case: not the best, not the worst
  - Maybe the pivot falls between $\frac{n}{4}$ and $\frac{3n}{4}$
  - Suppose partitioning always splits $\frac{n}{10}$ and $\frac{9n}{10}$
  - \# number of comparisons:
  - $$T(n)=\begin{cases}T(\frac{9n}{10}+T(\frac{n}{10})+cn\\0\end{cases}$$
  - Each level is once again $cn$ work. How many levels though?
  - $$\left (\frac{9}{10}\right )^hn\leq 1\implies h=\left \lceil\log\left (\frac{10}{9}\right )n\right \rceil\in O(\log n)$$
  - Thus, the running time is $O(n\log n)$
  - This isn't a proof though. Here is one, that $T(n)\in\Theta(n\log n)$:
    - $T(n)=\frac{1}{n}\sum\limits_{i=0}^{n-1}[T(i)+T(n-i-1)]+\Theta(n)$
    - Let $H(n)$ be the average height of the recursion tree.
    - $$H(n)=\begin{cases}1 + \frac{1}{n}\sum\limits_{i=0}^{n-1}\max[H(i),H(n-i-1)]&n\geq 2\\0&n\leq 1\end{cases}$$
    - $$H(n)\leq 1+\frac{1}{2}H\left(\frac{3n}{4}\right )+\frac{1}{2}H(n)$$
    - $$2H(n)\leq 2+H(\frac{3n}{4})+H(n)$$
    - $$H(n)\leq 2+H(\frac{3n}{4})\leq 2+2+H(\frac{9n}{16})$$
    - $\leq 2h$ where $h$ is minimal such that $(\frac{3}{4})^hn<2$
    - Thus, $H(n)\in O(\log n)$
    - Each level is $cn$ work, so the running time is $O(n\log n)$
    - Since the best cse is $\Theta(n\log n)$, the running time for the average case is also $\Theta(n\log n)$

