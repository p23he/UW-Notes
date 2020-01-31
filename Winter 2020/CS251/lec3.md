# Lecture 3

## Circuits

### Truth Tables
We can create circuits with $n$ inputs and $m$ outputs. We can specify the outputs for each possible input combination with truth tables. 
- long and hard to read

### Boolean Algebra

- Variables (usually $A, B, C$, or $X, Y, Z$) have values 0 or 1
- OR (+) operator has result 1 iff either operand has value 1
- AND ($\cdot$) operator has result 1 iff both operands have value 1
- NOT ($\neg A$ or $\bar A$) operator has result 1 iff the operand has value 0

Literals: a variable or its complement A, neg A

Mineral: a product of literals, one for each variable

### Going from truth table to formula using minterms

- Look for all the ones in the function in the truth table
- For each row, if the value is 0, add the complement
- Add up all the terms for the rows
- Issue is that the formulae can get long

### Two level representations
- Any Boolean functions can be represented as a sum of products (ORs of ANDs) of literals
- To create a product of sums, we look for all the 0s, and write the complement for every 1
- Take the product of the terms, the result is $\neg F$

### Don't cares in Truth Tables
- Represent $X$ instead of 0 or 1
- When used in output, indicates that we don't care what the output is for that input
- When used in input, indicates outputs are valud for all inputs created by replacing $X$ by 0 or 1
- Useful to show compressed truth tables and non-minimal terms

### Laws of Boolean Algebra
- There are many that are obvious
- E.g. $\neg\neg A=A$, DeMorgan's
- We can use algebraic manipulation (based on laws) to simplify formulae
- E.g.
  - $F=\bar X\bar YZ+X\bar YZ+XY\bar Z+XYZ$
  - $=\bar YZ(\bar X+X)+XY(\bar Z+ Z)$
  - $=\bar YZ+XY$

### Using Gates in Logic Design
- Converting the formulae to hardware
- Computers don't like AND and OR gates
- Sometimes there will be a question to create a circuit with only NAND gates - just put circles on IO

## Transistors and implementing gates
- Transistor: an electrically controlled switch
- An NMOS transistor (n-transistor)
  - Behaves like a normal switch
  - Transmits strong 0 but weak 1
  - That means if the input is 0, the transistor has low resistance. If the input is 1 there is high resistance, and it might transmit like only 0.8
- An NMOS NOT
  - In the input = 1 case, is there is a lot of current flow
- A PMOS
  - Opposite behaviour to NMOS - transmits strong 1 but weak 0
- CMOS
  - CMOS circuits use both n-transistors and p-transistors
  - If there is a path from power to ground AND power, you short circuit
  - If there are no paths, you have nothing
