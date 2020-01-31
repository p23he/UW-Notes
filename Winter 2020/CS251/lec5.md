# CS251 - Lecture 05, Jan 21

Sequential Circuits Continued

Projector not working today, whiteboard stuff.

## The D Latch

- Starts with an SR latch
- Put AND gates in front of the SR inputs
- The entire thing is the D latch
- What does it do?

|C|D|Next State of $Q$|
|---|---|---|
|0|X|No change|
|1|0|0 (Reset)
|1|1|1 (Set)

The point is, by putting those AND gates, we avoid the bad state (when $S=R=1$).

Is $C$ a control line or clock line? We would like it to be clock. We want memory to change only on the falling edge.

## D Flip Flop
Slide 31
- Have it set up so $Q_I$ is like $Q$ from the slide before ($Q_I$ copies $D$ when clock is _high_)
- Additionally, $Q_E$ will copy $Q_I$ when clock is _low_
- Insert image here
- The key point is that $Q_E$ only changes on falling edge of clock now

## Registers and Register Files
- Each flip flop stores one bit, and each register needs 64 bits
- Thus a register will be an array of flip flops
- Input is 5 bits since $5=\log_2(32)$
- Output 64 since we want 64 bit words
- We want to build the thing on slide 32

## Slide 33
- To read two registers, we read all of the registers, and use the selector input to decide which ones to use in the multiplexors


## RAM
- We want to build larger memories
- Register files don't scale as well
- We look at 3 types of RAM

### SRAM
- As long as power is on, it will save the info you gave it

## Three State buffer
- More transistors
- If $C=1$ $F$ will always get a strong signal from $X$
- If we're careful we can tie the outputs of 3 state buffers together

## XOR from three state buffers

|$X$|$Y$|$\bar X$|$\bar Y$|$F_0$|$F_1$|$F$|
|---|---|---|---|---|---|---|
|0|0|1|1|0|-|0
|0|1|1|0|-|1|1
|1|0|0|1|1|-|1
|1|1|0|0|-|0|0

If the control is 1, the output copies the input. If the control is 0, the output is floating.

Why might this version of the XOR be better than the last one? Because we have only 4 transistors! The previous one used 4 NAND gates = 16 transistors.

## Slide 40 not required

## Clicker!
The output of a D-flipflop changes
- on the falling edge of the clock

## Clicker!

## Clicker!
How man bits can you store on one flip flop? - 1

## MUX Question of the day