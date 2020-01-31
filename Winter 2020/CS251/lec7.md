# CS251 - Lecture 7, Jan 28

## Missed some stuff on implementing FSM
Slides 2-55
## Clickers
D-flops are used to store state variables

Given that we are in state 10, and given an input stream, find out what state we're in

End of this kind of stuff

## Building a Datapath
Our goal to is to build a datapath. We wold like to execute commands such as `ADD, CBZ, B, LDUR`. The pieces needed for the datapath:
- Register file (we need a place to store `X1,X2,...`)
- Adder (something to perform addition) / ALU (other mathematical functions)
- RAM
- MUXes: we need to send multiple outputs to a single input


## Sign Extension
In 4 bits, 1010 is -6. With 8 bits, -6 is 1111 1010.

## Addition Circuit
Slide 3-8

A|B|$C_{in}$|$C_{out}$|S
|-|-|-|-|-
0|0|0|0|0
0|0|1|0|1
0|1|0|0|1
0|1|1|1|0
1|0|0|0|1
1|0|1|1|0
1|1|0|1|0
1|1|1|1|1

$C_{out}=\bar ABC_{out}+A\bar BC_{out}+AB\overline{C_{out}}+ABC_{out}$ and 

$S=\bar A\bar BC_{in}+\bar AB\bar C_{in}+A\bar B\bar C_{in}+ABC_{in}$. This simplifies to
$$C_{out}=AB+AC_{in}+BC_{in}$$
and
$$S=A\oplus B\oplus C$$

## Logical Operations
- Left shift is move around
- Arithmetic is replace with 0