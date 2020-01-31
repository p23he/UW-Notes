# Lecture 4 - Jan 16

## Transistors Continued

Recall that we prefer to create NAND gates rather than AND gates with transistors in actual circuits, since they require less transistors.

## CMOS NAND
Consider the diagram on slide 54. How can we make this NAND gate into an AND gate? We cannot simply swap power and ground.

## CMOS 3 Input NAND and NOR
For $n$-input NAND: 2 transistors per input

## Short Circuiting
We never want a (low resistance) path to both power and ground from output. Then we would see a lot of current flow from power to ground.

We will need these tables:
#### NMOS
|Input|$A=0$ | $A=1$ |
|-|-|-|
|Resistance| High | Low

#### PMOS
|Input|$A=0$ | $A=1$ |
|-|-|-|
|Resistance| Low | High

Consider Slide 54 again, with this initial table.

|A|B|$Q_1$|$Q_2$|$Q_3$|$Q_4$|$Z$|
|---|---|---|---|---|---|---|
|0|0|Low|High|High|High|1|
|0|1|
|1|0|
|1|1|

Here is the analysis:

|A|B|$Q_1$|$Q_2$|$Q_3$|$Q_4$|$Z$|
|---|---|---|---|---|---|---|
|0|0|Low|High|High|High|1|
|0|1|L|L|H|L|1|
|1|0|H|H|L|H|- (Floating)|
|1|1|H||||Short Circuit (Disaster!)|

### Clicker
Consider Slide 53. If $A=1$, then $F$ connects to _ground_.

## Deriving Truth Table from Circuit

Consider slide 56. How do we fill in this table? Usually we will have to put the labels A,B, and C ourselves. Note that all the gates are NAND

Process:
1. Find gates all of whose input are known.
2. Compute its entries in the truth table.
3. Repeat (until F is found).

So first do A, then B and C, then finally F.
X|Y|A|B|C|F
|---|---|---|---|---|---|
|0|0|1|1|1|0
|0|1|1|1|0|1
|1|0|1|0|1|1
|1|1|0|1|1|0

Clicker: Answer is D. Answer is not A since the columns of $D$ are not correct. Finally check the $F$ column.

## Survey
My feelings about ARM programming are best described by
1. It's great!!!
2. It's enjoyable.
3. It was okay.
4. I disliked it.
5. The word "hate" is too mild to describe my dislike of ARm programming!!!

## Decoders
When we work on designing circuits, we need abstractions, not just AND and OR gates. The first useful thing is a _decoder_.

- Has $n$ outputs, $2^n$ outputs. 
- The example on slide 58 converts binary to decimal
- We will use decoders to pick one of $n$ things to turn off

## Multiplexors
- ***Important!!!!!!***
- Based on the select inputs, it will output one of the $D$ inputs.
- Strange since we have inputs in our output column, but this way of writing the table is more descriptive.
- Another way of writing the truth table below (tedious).
- Used to select stuff when stuff is happening in parallel

$D_3$ | $D_2$ | $D_1$ | $D_0$ | $S_1$ | $S_0$ | Y
|---|---|---|---|---|---|---|
|0|0|0|0|0|0|0|
|$\vdots$|$\vdots$|$\vdots$|$\vdots$|$\vdots$|$\vdots$|$\vdots$|

### Clicker - B, use table

### Clicker - A, Idk why

## ROM
We care about ROM since it's needed to start things up and a computer turns on.
- ROM cannot be written to
- not much else

## Second type of circuit - sequential
- Two types of sequential circuits

## SR latch with NOR Gates
- Asynchronous circuit example
- How do we analyze this? We cannot fill the 0-0 row in.
- Notice that if one of the inputs are 1, then can fill in the rest of the table

|S|R|$Q$|$\bar Q$|
|---|---|---|---|
|0|0|?|?|
|0|1|0|1|
|1|0|1|0|
|1|1|0|0|

If SR=10 and changed to 00, 

|$S,R$ transistion|$Q,\bar Q$ transition|
|---|---|
|$1,0\to 0,0$|$10\to10$|
|$0,1\to 0,0$|$01\to01$|
|$1,1\to 0,0$|$00\to$ BAD|