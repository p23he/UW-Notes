# CS240 - Lecture 8, Jan 30

## Decision Trees
Comparison-based algorithms can be expressed as **decision trees**.
- Use comparisons to distinguish between outcomes
- If there are $n=3$ items in the input array, there are $n!=6$ possible permutations of the array
- The comparisons are $<, \geq$, which means each node of the tree has 2 children
- To do one more comparison, the original leaf node becomes an interal node, and we get two more leaves (net result is one more leaf)
- Thus $n!$ leaves $\implies n!-1$ internal nodes
- We need to find the height of this tree:
  - Total number of nodes: $2n!-1$
  - Best case, tree is balanced: 
  - $$\log(2n!-1)=\log(n!)$$
  - $$=\log(n)+\log(n-1)+\dots+\log(2) $$
  - $$\geq\log(n)+\dots+\log(n/2)$$ 
  - $$\geq \log(n/2)+\dots+\log(n/2)$$
  - $$=(n/2)\log(n/2)\in\Omega(n\log(n))$$

Theorem: Any correct *comparison-based* sorting algorithm requires at least $\Omega(n\log n)$ comparison operations to sort $n$ distinct items.

## Non-comparison based sorting
Assume keys are numbers in base $R$ (radix)
- $R=2,10,128,256$ are the most common

Assume all keys have the same numner of $m$ digits
- Use leading 0s

Can sort based on individual digits
- How to sort 1-digit numbers?
- How to sort multi-digit numbers based on this?

### Bucket Sort (Single Digit)
Sort an array $A$ by last digit. fast but big

### Count Sort
- idx ~ size $R$
- aux ~ size $n$
- Extra spare $\Theta(n+R)$

Pseudo code runtime:
3R + 3n = $\Theta(n+R)$

### MSD - Radix Sort
Say we have $n=7, R=10$.

$A[829, 331, 457, 330, 436, 720, 355]$

We order by the first digit, put em into buckets

$[331, 330, 355], [457, 436], [720], [829]$

Now sort each bucket recursively, go to the next digit.

At the end, each number has its own bucket, and how we have our sorted order. Each pass we had to call counting sort, and we did 3 (the number of digits) passes. 

Consider the keys in the range [0, 999999]

R = 10 ~ decimal digit, m = 6 digits

$\Theta(m(n+R))=\Theta(6(n+10))$

R = 1000, "digits are 0...999" m=2, $\Theta(2(n+1000))$

How about R=4, m=3. We could have done $R=1000_4$ and digits:0...$333_4$ and m = 1