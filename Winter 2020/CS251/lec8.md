# CS251 - Lecture 8, Jan 30

## Finite State Diagrams Review
- There are often times where there are unused states
- We must ensure that these states will not be reached

## Back to Adding numbers - Logical Operations

Last time we were talking about rotate.
- When we left all the bits to the left, and the left most bit goes to the right end

Another choice we had was to to do an *arithmetic shift left*
- Same thing as rotate left, but throw away left most bit and add a 0 on the right

When doing 2's complement, when we do an arithmetic shift right, we copy the top bit on the left

### Bitwise operations - AND, OR etc
- When using AND on two binary numbers, AND each bit
  - e.g. 1001 AND 1000 = 1000
- Similarly with OR, NAND, and XOR


### MIdtem question
How we we build and ALU that could output 0: all 64 $R_i$ bits are zero
- We use a NOR gate

Why do we care about 0?