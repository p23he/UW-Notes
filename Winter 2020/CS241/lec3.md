# Lecture 03 - Jan 14th 2020

## Last class
- Fetch - Execute Cycle
  - Loader
    - loads program into RAM
    - at 0x0
    - Sets the PC to 0x0
    - Loads up $31 with the return address
  - PC (Program Counter)
    - Stores the memory address of the next instruction
  - IR (Instruction Register)

## MIPS code
A program that adds two ints from register 1 and 2, and stores the result in register 3 is:
```MIPS
add $3, $1, $2
jr $31
```
What happens in memory:
```
Address Memory (RAM)
0x0000 binary
0x0004 binary
```

## Load Immediate and Skip
MIPS reference sheet: `lis $d`

Instead of specifying a memory address to load from, `lis` loads the next word in memory into the destination register and then skips to the word after that.

Example:
```MIPS
lis $7
.word 0x7
```
To execute `lis $7`, thr `.word 0x7` at the current PC is loaded. Then PC$\leftarrow$PC + 4.

Example: Program that adds 27 to 42 and stores the sum in $3.

```MIPS
lis $1
.word 0x1b ; .word 27 works
lis $2
.word 42
add $3, $1, $2
jr $31
```
Note: you should always return, even if it's not explicitly said to.


### Another example:
|Address|Assembly|Hexadecimal|
|---|:---:|---|
|0x0000|`lis $1`|0x0000 0814|
|0x0004|`lis $2`|0x0000 1014|
|0x0008|`jr $0`|
|0x0012|`jr $31`|

The value 0x0000 1014 is loaded in register $1 (in decimal it's 4116). The program is an infinite loop.

## Branching
Compares contents of two registers; if true, branch, i.e. modify PC by the given offset number of words.

MIPS reference sheet: `beq $s, $t, i` where `i` is the integer offset (unit is number of words, programmer does not need to multiply by 4).

What does `beq $0, $0, -1` do? This goes back to the same instruction, and loops forever.

## Set Less Than
There are two forms of this command: set less than and set less than unsigned. It compares the contents of two registers (as either two's complement or unsigned numbers); sets destination register with result.

MIPS reference sheet: `slt $d, $s, $t`

### Example program
Write a program that computes the absolute value of `$1` and store the result in `$1`.
```MIPS
slt $2, $1, $0 ; check if $1 < $0, store flag in $2
beq $2, $0, 1 ; if the flag is 0, skip the next line
sub $1, $0, $1 ; store negation of number in $1
jr $31 ; return
```

### Loop example
Write a program that adds all the even numbers from 2 to 20 (inclusive) and stores the sum in register `$3`

```MIPS
lis $1 ; stores current even number
.word 20
lis $2 ; store the number 2
.word 2
add $3, $0, $0 ; initialize sum
add $3, $1, $0 ; add current even number, top of loop
sub $1, $1, $2 ; decrement current even number
bne $1, $0, -3 ; loop back to the adding line
jr $31
```

## Multiplication and Division
MIPS reference sheet: `mult $s, $t` and `div $s, $t`. Where is the destination register?

Many bits are required for the product of two 32-bit numbers. The results are stored in special registers `hi` and `lo`. In this course, multiplcation will always store the result in `lo`.

Accessing `hi` and `lo`: `mfhi $d` and likewise for `lo`.

### Example
Give `$1` stores the base address of an array and `$2` stores a valid index, write a program that loads the value into `$3`.

Recall that `a[i]=*(a+i)` where `i` is multiplied by 4.

```MIPS
lis $4 ; load the value 4
.word 4
mult $2, $4 ; multiply the index by 4
mflo $5 ; store the result in register 5
add $6, $1, $5 ; get the address of the value
lw $3, 0($6) ; store the result from memory into register 3
jr $31
```

### Example
Write a program that checks if $2 evenly divides $1. If true, $3 $\leftarrow$ 1; otherwise $3 $\leftarrow$ 0. Registers $1 and $2 must remain unchanged.

```MIPS
div $1, $2
mfhi $3
bne $3, $0, 4 ; if remainder != 0 branch
lis $4 ; case 1, put 1 in register 3
.word 1
add $3, $4, $0
beq $0, $0, 1
add $3, $0, $0 ; case 2, put 0 in register 3
jr $31
```

## Assembly Language
Replaces the binary encoding of mcahine language instructions with easier to use mnemonics; i.e. its more English like code.

Allows for comments and extra whitespace. 1 line of assembly is one line of machine code.

### Labels
Assemblers allows programmers to label instructions and to use the labels within the assembly language instructionso programmers do not have to manually calculate jump addresses or branch offsets

### Revisiting loops
``` MIPS
lis $1
.word 20
lis $2
.word 2
add $3, $0, $0
top:
add $3, $3, $1
sub $1, $1, $2
bne $1, $0, top
jr $31
```
top is assigned memory 0x14
Assembler computes (top/PC) / 4=0x14-0x20 / 4=-3

```MIPS
slt $2, $1, $0
beq $2, $0, foo
sub $1, $0, $1
foo: jr$31
```