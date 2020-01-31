# CS251 - Lecture 6, Jan 23

## Finite State Machines

State | Inputs | Next State
|-|--|-|
|NSGreen|| NSGreen|
| | EWCAR| EWG|
| | NSCAR | NSG|
| | NSCAR, EWCAR| EWG|
|EWG|  1100


Output table:
State|Output (NSL, EWL)
|-|-|
|NSG|1,0|
|EWG|0,1|

Boolean Formula for State table above: $S'=\bar S\overline{NSC}EWC+\bar SNSCEWC+ S\overline{NSC}\overline{EWC}+S\overline{NSC}EWC = \bar SEWC+S\overline{ NSC}$

## State Table of Extended Controller

Current State $S_2 S_1 S_0$ | Input NSC, EWC, TS | Next State $S_2'S_1'S_0'$
|-|-|-|
000|XX0|000|
000|XX1|001|
001|X0X|001|
001|X1X|010|
010|XXX|100|
011|XXX|XXX|
100|XX0|100|
100|XX1|101|
101|0XX|101|
101|1XX|110|
110|XXX|000|
111|XXX|XXX|

### Output Table of Extended Controller

$S_2S_1S_0$|$NS_g, NS_y, NS_r, EW_g, EW_y, EW_r, TR$|
|-|-|
000|1000010|
001|1000010|
010|0100011|
011|XXXXXXX|
100|0011000|
101|0011000|
110|0010101|
111|XXXXXXX|