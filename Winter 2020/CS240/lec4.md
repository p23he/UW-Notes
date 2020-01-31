# CS240 - Lecture 4, Jan 16

## Last Class
We went through some techniques to analyze code and come up with a running time in $\Theta$ notation. We had two methods after coming up with a sum: a direct method (maintaining equality), and a sloppy method (finding $O$ and $\Omega$).

Apparently the sloppy method can be faster if you're quick :O


## Slide 32:

Let $T_{\mathcal A}(I)$ denote the running time of an algorithm $\mathcal A$ on instance $I$.

Test 3 - Insertion Sort. Lets find the worst case: the runtime on worst possible input of size $n$

- reverse sorted order
  - 1+2+3+...+n-1$\in\Theta(n^2)$

Best case?
- already sorted
  - Each iteration is only 1 comparison
  - $\in\Theta(n)$

Big O does not mean worst case! It means an upper bound.

Average case: the average run time over all inputs of size $n$.
- The average run time for insertion sort is $\Theta(n^2)$

## Slide 34:

Matrix Multiplication: Consider $A_2$, which is an algorithm with running time $2n^3-n^2\in\Theta(n^3)$

Now consider $A_1$: the Strassen method, which has a running time $42n^{2.8074\dots}$. Which one is faster?

## Slide 36:

### Divide and Conquer Algorithm
- Always recurse on a smaller subproblem!
- We know that merging is $\Theta(n)$

## Slide 39
  It can be shown that $T(n)\in\Theta(n\log n)$ if $n=2^j$ for some $j$

### Method 1
Expand recurrance "Plug and Chug"

$$T(n)=2T(n/2)+cn$$
$$=2[2T(n/4)+c(n/2)]+cn$$
$$=2[2[2T(n/8)+c(n/4)]+c(n/2)]+cn$$
$$=8T(n/8)+c[4(n/4)+2(n/2)+n]$$
$$\dots$$
$$=nT(1)+c[n+2(n/2)+4(n/4)+\dots+(n/2)\left(\frac{n}{n/2}\right)]$$
$$=n\cdot c+c\cdot n(\log n-1)\in\Theta(n\log n)$$

## Module 02

## Heap - Binary Tree Implementation
- Not a BST

Shape: Structure property - nearly complete Binary Tree
- The last level may not be full
- The root has the highest priority

Let the height of a node be the number of edges in the longest simple path down to a leaf.

Lemma: The height of a heap with $n$ nodes is $\Theta(\log n)$.

Proof: Suppose the height of a heap is $h$. Since each level except the last is full,
$$n\geq 2^0+2^1+\dots+2^{h-1}+1$$
$$n\geq 2^h$$
$$h\leq \log n$$

so $h\in O(\log n)$. Furthermore,

$$n\leq 2^0+2^1+\dots+2^{h-1}+2^h$$

$$n\leq 2^{h+1}-1$$
$$h\geq \log(n+1)-1$$

so $h\in\Omega(\log n)$. Thus, $h\in\Theta(\log n)$. QED