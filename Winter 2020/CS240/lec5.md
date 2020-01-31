# CS240 - Lecture 05, Jan 21

## Last Class
Recall that a **binary tree** is either
- empty, or
- consists of three parts: a node and two binary trees (left and right subtree)
- A binary tree with $n$ nodes has height at least $\log(n+1)-1\in\Omega(\log n)$

## Priority Queue
Heap:
- structure: Binary Tree (not a BST though)
- order property
- Last level may not be full
- Height: $\Theta(\log n)$, where $n$ is the number of nodes

We will use a heap to implement a priority queue.

### Example (Max) Heap:
                    50
                /       \
                29          34
            /       \       /   \
        27          15      8   10
        /\
    23    26  
In these examples, only the priorities will be shown, and they are shown directly in the node.

## Implementation:
Option 1: Dynamically allocated nodes (on the heap) linked together.

Option 2: An array. We use this option since it's more space efficient.
- Since almost all levels are full, an array would use almost all the indices, with little waste

Let $H$ be a heap of $n$ items and let $A$ be an array of size $n$. Store the root in $A[0]$ and continue with elements *level by level* from top to bottom in each level left to right.

                    a0
                /       \
                a1          a2
            /       \       /   \
        a3          a4      a5   a6
        /\
    a7    a8  

### Navigation
It is easy to navigate the heap using this array representation:
- The root is at index 0
- Left child of node $i$ is node $2i+1$
- Right child is $2i+2$
- Parent of node $i$ is node $\lfloor \frac{i-1}{2}\rfloor$
- Last node is $n-1$

We should hide implementation details using helper functions:
- functions `root(), parent(i), last(n)` etc.

### *Insert* in Heaps
- Place the new key at the first free leaf
- The heap-order property might be violated: perform a *fix-up*

We would like to insert so that structure is maintained, and then we would need to fix the order.
```
fix-up(A,k)
k: an index corresponding to a node of the heap

while parent(k) exists and A[parent(k)].key < A[k].key do
    swap A[k] and A[parent(k)]
    k <- parent(k)
```
The new item "bubbles up" until it reaches its correct place in the heap.

Time: $O($height of the heap$)=O(\log n)$

### *deleteMax* in Heaps
- The maximum item of a heap is just the root node
- We replace root by the last leaf (last leaf is taken out)
- The heap-order property might be violated: perform a *fix-down*

```
fix-down(A,k)
k: an index corresponding to a node of the heap

```

## Sorting PQs using heaps
Recall that any pq can be used to sort in time $O($initialization + $n\cdot$ insert + $n\cdot$ deleteMax$)$

Using the binary-heaps implementation of PQs, we obtain:

```
PQsortWithHeaps(A)
1.  initialize H to an empty heap
2.  for k <- 0 to n-1 do
3.      H.insert(A[k])
4. for k <- n-1 down to 0 do
5.      A[k] <- H.deleteMax()
```

Both operations run in $O(\log n)$ time for heaps $\implies$ PQ-sort using heaps takes $O(n\log n)$ time.

## Building Heaps with FixUp
Problem: Given $n$ items all at once (in $A[0\dots n-1]$) build a heap containing all of them.

Soltution 1: Start with an empty heap and insert them one at a time ($\Theta(n\log n)$)

### Heapify (Bottom-up)
Suppose we have array $A:3,1,2,6,5,0$

Just based on our array, we have

                    3
                /       \
                1          2
            /       \     /
          6          5   0

Order is a mess, but structure is fine. How do we fix order?

Start at `parent(last(n))`
- Last node in array that's not a leaf


# Random Algorithms

## Selection vs Sorting
