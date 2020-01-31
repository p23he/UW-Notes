# CS241 Lecture 4 - Jan 16

## Assigning Memory Addresses to Labels
- Remember that whitespace, comments, and labels are for programmers to more easily read, write, and organize code. They do not get translated in machine code!

When assigning a memory address to a line laebl:
- Blank lines are simply stripped out
- Whitespace after labels is removed
- Only instructions (or .word) are assigned addresses
- A lavbel is assigned to the memory address of the instruction that follows it
- A label may appear at the end of your code and will be assigned the memory address of the word after your program

Recall (from CS136): What happens when you call a function?
- copy the arguments into parameters
- parameters and local variables are in a stack frame
- jump to the code for that function
- execute body of the function
- return back to where called from

## Implementing Procedures in MIPS
All those little things you take for granted in a high lebvel language must be implemented!!

Procedures in MIPS are a bit different:
- All procedures share the same set of registers
- Procedures do not return values

Logistics: How to call and return from a procedure

Problem: How to share registers?
- A caller may have critical data stored in a register that the callee should not overwrite!

## Sharing Registers
Strategy: guarantee that when a procedure ends, the values in the registers are the same as when the procedure was called. 

Where can we save register data?
- Two places we typically store data is in memory (RAM) or registers (CPU)
- It would be nice to save everything in registers (fast access, etc) byt space is very limited
- If we use registers, we may run out

Where does C/C++ store data? In the **stack frames**.

## Storing on the Stack

|`0x0`$\to$code|
|---|
|read-only|
|global variable|
|heap|
|$\downarrow\\\uparrow$|
|stack$\leftarrow$ `$30`|

  Loader
  - finds block of memory and loads program
  - PC $\leftarrow$ 0x0
  - `$31` $\leftarrow$ return address to loader
  - `$30` stores the memory address of bottom of block allocated

Recall: a loader allocates a block of RAM (larger than program) and loads our program at the top of the block, then sets the PC$\leftarrow$ 0. It also sets $30 to the memory address immediately following the allocated block.
- Use $30 to store address of the top of the stack
- Grow stack from high memory addresses to low

For example: if procedure `f` called `g` and `g` calls `h` then
- `f` stores register data at the bottom of the stack
- `g` stores register data above `f`
- `h` stores register data at the top of the stack

Strategy: each time a procedure is called, it will save the current value stored in the registers it wants to use on the stack and restore the original values when it ends.
- Only need to save registers that the procedure will overwrite. If in doubt, save everything.
- Remember registers are 32 bits/4 bits
- Remember to increment (decrement) $30 when you push (pop)
- Remember the order you placed items on the stack
- Careful of "off by errors"

## Storing on the Stack
To save a value from the register onto the stack, we use the `sw` command
```MIPS
sw $7, -4($30) ; say we want to save register 7
lis $4
.word 4
sub $30, $30, $4
```

## Template for Procedures
Suppose procedure `f` modifies register $1 and $2:
```MIPS
f: sw $1, -4($30) ; push register f modifies
sw $2, -8($30)
lis $2 ; decerement stack pointer
.word 8
sub $30, $30, $2
; Body of your procedure goes here
lis $2 ; increment stack pointer
.word 8
add $30, $30, $2
lw $2, -8($30) ; pop registers to restore
lw $1, -4($40)
; when in doubt save all registers
; How do we return?
```

## Calling and Returning
Label `f` represents the memory address of a procedure `f`.
``` MIPS
main:
    lis $5
    .word f
    jr $5
    ; RETURN HERE
    ...

f: ...
```
How do we know the memory address where `f` returns to?

## Returning - `jalr`
MIPS reference sheet: `jalr $s`

Last instruction: Jump and Link Registers

Copies PC into `$31` then jumps to address stored in $s.

Don't forget, we must have the original value of $31 somewhere first, before we jump.
- $31 is special - it stores the memory address (in the loader program) we jump back to when our program ends.

Who saves $31? Procedure `f`?
- We need to save it before we jump to `f`.
- The caller savers $31 first, then calls the procedure.

### Calling `f`
```MIPS
sw $31, -4($30)
lis $4
.word 4
sub $30, $30, $4
lis $5
.word f
jalr $5
lw $31, 0($30)
add $30, $30, $4 ; decrement stack pointer
```

## Parameters and Result Passing
- Simple approach: use registers (Document!)
- If too many parameters, can use memory (stack)

Example: Procedure `sumEvens2ToN`
```MIPS
; sumEvens2ToN: adds all even numbers from 2 .. N
; Requires: N is even
; Registers:
;   $1 - Temporary work
;   $2 - Parameter N
;   $3 - Sum to return
```
Which registers should be saved? We should save $1, since by definition we said we're going to overwrite it. $2 is a toss up (usually yes, maybe no). Do not save $3.

```MIPS
sumEvens2ToN:
    sw $1, -4($30) ; save registers $1 and $2 on the stack
    sw$2, -8($30)
    lis $1
    .word 8
    sub $30, $30, $1 ; decrement stack pointer
    add $3, $0, $0 ; Initialize sum <- 0
    lis $1 ; use temporary work register
    ...
    lis $1 ; restore $1 and $2
    .word 8
    add $30, $30, $1
    lw $2, -8($30)
    lw $1, -4($30)
    jr $31 ; jump back to caller

```

## Printing to stdout
Use `sw` to store a word in address `0xffff000c`. Least significant byte will be printed to `stdout`.

Example: Write a program that prints "CS\n" followed by a newline
```MIPS
lis $1
.word 0xffff000c
lis $2
.word 67 ; ASCII for 'C'
sw $2, 0($1)
.word 83 ; ASCII for 'S'
sw $2, 0($1)
lis $2
.word 10 ; ASCII for \n
sw $2, 0($1)
jr $31
```

## Reading from stdin
Use `lw` in the same manner as `sw`.

## The Assembler
Goal: Automate the translation of assembly code to machine code

Input: Assembly Source code

Output: Machine code

Translation has 2 phases:
1. Analysis: Understsand the meaning of source string
2. Synthesis: Output the equivalent target string

## Assembly Translation
Read the input one ASCII char at a time; i.e., as a stream of char.

The first step is to group characters into meaningful **tokens**:
- Labels, register \#, hex \#, `.word`, etc.
- Note this is done for us in `asm.rkt` and `asm.cc`

Our job:
1. Analysis: Check sequence of tokens is a valid program
2. Synthesis: Output equivalent machine code

Focus on checking if the seuqnece of tokens is valid; anything else, output an error message containing the word ERROR to **stderr**.