# CS241 - Lecture 5, Jan 21

## Slides 5, Slide 1
What instruction does the loader use to start execution of a user program?
- Finds block of memory > size of user program
  - Loads to 0x0
  - PC $\leftarrow$ 0x0 (requires `jalr $0`)
  - $31 $\leftarrow$ return address (requires `jalr $0`)
  - $30 $\leftarrow$ last memory address of a block

## Assembler Challenges
Most of the process is straightforward since 1 assembly instruction translates to exactly 1 machine language instruction.

Challenge (Extra things our Assembler does):
- Commetns and whitespace are simply discarded
- Labels are used to compute memory addressed for jumps and branch offsets

### Labels
We want to read 1 assembly instruction and directly output its encoded machine instruction

How to assemble:
```MIPS
       beq $0, $1, label
       ...
label: add $22, $10, $31
```
**Problem:** To encode `beq` we need the memory address of `label`, but we haven't encountered this label yet!

## 2-Pass Assembler
Pass 1:
- Group tokens into instructions, verifying instructions are valid (for example max input bits)
- Keep track of the memory address (starting at `0x0`) each instruction will be given when loaded into memory
- Build a **symbol table** for (label, address) pairs (use `map`)
- **Note:** 
  - multiple labels may have the same address
  - labels can only be defined once

Pass 2:
- Translate each instruction into machine code
- If a label is encounteed, look up associated address,

## Symbol Table Example
```MIPS
0x00    main:   lis $2
0x04            .word 20
0x08            lis $1
0x0c            .word 2
0x10            add $3, $0, $0
        top:
0x14            add $3, $3, $2
0x18            sub $2, $2, $1
0x1c            bne $2, $0, top
0x20            jr $31
0x24    beyond:
```
|label|address|
|---|---|
|`main`|`0x00`|
|`top`|`0x14`|
|`beyond`|`0x24`

Recall, offset in `bne: (top - PC) / 4 = (0x14 - 0x20) / 4 = -3`
## Encoding Instructions Into Binary
Translate each assembly instruction into its binary encoding
```MIPS
Avengers: lis $2
          .word Avengers
```

**Assemble!**

`lis $2` $\implies$ `0x00001014`
`.word 0x0` $\implies$ `0x00000000`
`bne $2, $0, top` $\implies$ `0x1440fffd`
- `bne` has op code `000101`
- 2 $\implies$ `00010`
- 0 $\implies$ `00000`
- `top=-3` $\implies$ `1111111111111101=0xfffd`

## Assembling the Pieces
Obtain the pieces from the sequence of okens, then assemble!

Assembly: `bne $2, $0, -3`

Binary: `0001 0100 0100 0000 1111 1111 1111 1101`

Can we simply print out each piece, token by token?
- `printf("000101); printf("00010"); ...`
- `printf("0x"); printf("1"); printf("4")...`

No!

## Assemblying the Pieces
We need to build and store the encoded instruction using 32 bits, then output the result.

What `type` in C++ can we use that has 32 bits? **int**

How do we put the first piece into place? The first piece should be `000101=5`.

#### Bitwise operators!
How far do we need to *shift*?

(int) 5 is :

`0000 0000 0000 0000 0000 0000 0000 0101` while we want:

`0001 0100 0000 0000 0000 0000 0000 0000`

```MIPS
bne $2, $0, -3
```
opcode `000101=5`(int),  5 << 26

Similarly, we do `2 << 21` to shift register `$2`

Our final result is 
```C++
int instr = (5 << 26) | (2 << 21) | (0 << 16) | (-3 & 0xffff); // instr = 339804157
```

Now, we cannot simply `cout << instr`. We must print each character byte by byte:
```C++
int instr = 339804157;

char c = instr >> 24;
cout << c;
c = instr >> 16; // shifting byte 2, c = 24, ASCII for @ sign
cout << c;
c = instr >> 8; // c = 255
cout << c;
c = instr; // c = 253
cout << c;
```
