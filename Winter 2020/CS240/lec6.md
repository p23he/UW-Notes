# CS240 - Lecture 6

## Quickselect
- Given index $k$, find item that would be at $A[k]$ if array $A$ was sorted
- Choose pivot $p$
- $i=$ partition $\left (A,p\right )$
  - In place (Hoare)
  - Pivot belongs at index $i$
- Compare $i$ with $k$
    1. $i=k$ done
    2. $i>k$ search $L=A[0,\dots,i-1]$ for index $k$
    3. $i < k$ search $R=a[i+1,\dots,n-1]$ for index $k-i-1$
- In the best case, $i=k$ after 1 iteration
  - $\Theta\left (n\right )$
- In the worst case, we found that the time is $\Theta\left (n^2\right )$

## Sorting Permutations
- Need to take average running time over all inputs
- How to characterize input size $n$?
- Simplifying assumption: all input numbers are distinct
- The actual numbers do not matter, only their relative order
- Characterize input via sorting permutation: the permutation that would put the input in order
- Assume all $n!$ permutations are equally likely
- Average cost is sum of costs for all permutations divided by $n!$

## Average-Case Analysis of *quick-select1*
- Define $T\left (n\right )$ to be the average cose for selecting from size $n$ array
- Fix one $0\leq i\leq n-1$. There are $\left (n-1\right )!$ permutations for which the pivot index is $i$

### Theorem: $T\left (n\right )\in\Theta\left (n\right )$
Proof:

$A=[\dots|\dots|\dots]$ where the first bar is position $\lfloor \frac{n}{4}\rfloor$ and the second bar is at $\lfloor\frac{3n}{4}\rfloor$

The probability that $i$ is in the middle is $\frac{1}{2}$. 

- The subproblem size is at most $\lfloor \frac{3n}{4}\rfloor$

Similarly, the probability that $i$ is not in the middle is $\frac{1}{2}$.
- The subproblem size at most $n$

$$T\left (n\right )\leq\begin{cases}cn+\frac{1}{2}T\left (n\right )+\frac{1}{2}T(\frac{3n}{4})\\d&n\leq 1\end{cases}$$

$$T\left (n\right )\leq cn+\frac{1}{2}T\left (n\right )+\frac{1}{2}T\left (\frac{3n}{4} \right )$$
$$2T\left (n\right )\leq2cn+T\left (n\right )+T\left (\frac{3n}{4}\right )$$
$$T\left (n\right )\leq2cn+2T\left (\frac{3n}{4}\right )$$
$$T\left (n\right )\leq2cn+2c\left (\frac{3n}{4}\right )+T\left (\frac{9n}{16}\right )$$
$$T\left (n\right )\leq2cn+2c\left (\frac{3n}{4}\right )++2c\left (\frac{9n}{16}\right )+T\left (\frac{27n}{64}\right )$$
$$\dots$$
$$T\left (n\right )\leq d+2nc\sum_{i=0}^\infty \left (\frac{3}{4}\right )^i$$
$$T\left (n\right )\leq d+8nc\in O\left (n\right )$$

QED

## Randommized Algorithms
A randomized algorithm is one which relies on some random numbers in addition to input
- The run-time will depend on th einput and the random numbers used

Consider this function:
```
foo()
    k <- roll a 6-sided die
    for i=1 to k
        print("*")
```

The expected number of calls to print is 
$$\sum_{i=1\dots 6}\left(\frac{1}{6}\right)i=\frac{7}{2}$$

Let $A[0\dots n-1]$ be a permutation of $1,1,\dots, n$

```
foo2(A,n)
    k=0
    for i=1 to A[k]
        print("*")
```

Best case: $\Theta(1)$, worst case: $\Theta(n)$
- There are instances where these results occur

Average run time: There is one case where $n$ items are printed, and $n-1$ cases where 1 item is printed. Thus:

$$n(\frac{1}{n})+\frac{n-1}{n}(1)\in $$

Now consider
```
foo3(A,n)
    k <- random(0, ..., n-1)
    for i=1 to A[k]
        print("*")
```

- There are no good