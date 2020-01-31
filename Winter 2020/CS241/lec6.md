# CS241 - Lecture 6, Jan 23

## Formal Languages
Definitions
- Alphabet - non-empty finite set of symbols, denoted $\Sigma$
- String (or word) - finite sequence of symbols from $\Sigma$
- Language - set of strings

String
- Denote the length of a string $w$ by $|w|$
- Empty string denoted by $\epsilon$, $|\epsilon|=0, |\epsilon\epsilon\epsilon|=0$. Note: $\epsilon$ is not a symbol in $\Sigma$

### Examples:

Is $\epsilon\equiv\epsilon\epsilon\epsilon$? Yes.

For example: Given $\Sigma=\{a\}$, is $a=\epsilon a\epsilon$? Yes.

$\{\}$ or $\emptyset$ is the empty language. What about $\{\epsilon\}$? This is a singleton set. Language with 1 string - only contains $\epsilon$.

$\{\epsilon,\epsilon\epsilon\}$ is a set of one item.

The alphabet will depends on the domain you are working with.
-$\Sigma=\{\text{ASCII characters}\}, L=\{\text{Valid C programs}\}$

Problem: Given a string, how to recognize if it belongs in a specific language.

## Recognition
The difficulty of determining if a string belongs to a specified language depends on the complexity of the language
- English words - Easy
- Valid Assembly programs - Still easy
- Valid C programs - harder
- Valid C programs that always halts - impossible

## Classes of Languages
The difficulty of recognition defines a hierarchy oof classes of languages:
- Finite (easy)
- Regular
- Contextfree (harder)
- Context sensitive
- Recursive (hard)
- Recursively enumerable
- etc (impossible)

## Methods of Recognition
How can we determine if a given string belongs to a language?
- Write a program (based on the language) to check
- Other - finite state machines, Turing machines, etc.

For example: Given language $L=\{$bat, bag, bit$\}$

We could write a program to check if a given input string matches one of the words within the language.

### Aside: Finite State Machines
- Read input from left to right
  - one char at a time
- never go back in input
- Must read all of the input

### Extremely Important Features of Diagram
- Circles represent *states* - these represent the state of the program; i.e. the progress we have made in recognizing a valid patterns recognized so far
- An arrow into the start state denotes where to bein
- Transition arrows from state to state have a symbol symbole
- States that accept are double circled

## Regular Languages
- Finite languages
- Union: $L_1\cup L_2=\{x:x\in L_1\text{ or }x\in L_2\}$
- Concatenation: $L_1\cdot L_2=L_1L_2=\{xy:x\in L_1, y\in L_2\}$
- Kleene star $L^*=\{\epsilon\}\cup\{xy:x\in L^*,y\in L\}=\bigcup\limits_{n=0}^\infty L^n$ where

$$L^n=\begin{cases}\{\epsilon\}&n=0\\LL^{n-1}&\text{otherwise}\end{cases}$$
- Equivalently, $L^*$ is the set of all strings consisting of 0 or more occurances of strings from $L$ concatenated together

### Example
Consider the set $\{a^{2n}b:n\geq 0\}$. This set is regular. The set $\{\epsilon, aa, aaaa,\dots\}$ can be represented by $\{aa\}^*$, so our first set is represented by $\{aa\}^*\cdot\{b\}$.

### Regular Expressions
Regular Expression | Set Notation | Description |
|-|-|-|
|$\emptyset$|$\{\}$ | Empty Language
|$\epsilon$| $\{epsilon\}$ | Language only containing $\epsilon$
|a|$\{a\}$| Singleton language of 1 string
|$E_1$\|$E_2$| $L_1\cup L_2$ | Alternation
|$E_1\cdot E_2$|$L_1L_2$|Concatenation
|$E^*$|$L^*$|Repetition

Precedence:
$^*$ before $\cdot$, e.g. $aa^*\equiv a(a^*)$

$\cdot$ before $|$, e.g. $aa | b\equiv (aa)|b$

$E=(aa)^*\cdot b$ - Regular Expression, $L(E)=$ Language of Expression $E$

## Deterministic Finite Automata (DFA)
Formally a DFA $M$ is a 5-tuple $M=(\Sigma, Q, q_0, A, \delta)$
- $\Sigma$ - non-empty, finite alphabet
- $Q$ - non-empty, finite set of states
- $q_0$ - start state
- $A\subseteq Q$ - set of accepting states
- $\delta:(Q\times \Sigma)\to Q$ transition function

From a given state, on a given alphabet symbol transition to next state - consumes single symbol.

### Extended Transition Function
We can extend $\delta$ to consume a word (instead of a single symbol) by the following recursive definition:

Base case: $\delta^*(q,\epsilon)=q$

Recursive case: $\delta^*(q, cw)=\delta^*(\delta(q,c), w)$ where $c\in\Sigma, w\in\Sigma^*$

**Acceptance**: DFA $M$ accepts a word $w$ if $\delta^*(q_0,w)\in A$, i.e., accept if starting at $q_0$ and following $\delta$ for each symbol of $w$, in turn, ends in a state in $A$.

The language (set) of $M$ is all words ...

### Example
Say we have a language $L=\{w\in\Sigma:w$ contains $abb$ as a substring$\}$. Can we write a regex for this set of strings? It is implied that $\Sigma=\{a,b\}$.

Regex: $E=(a|b)^*abb(a|b)^*$.

Another: $L_3=\{w\in \Sigma:w$ contains $abb$ or $ba$ as a subtring$\}$.

Regex: $\\(a|b)^*abb(a|b)^*ba(a|b)^* | \\(a|b)^*ba(a|b)^*abb(a|b)^*|\\ (a|b)^*(babb|abba)(a|b)^*$ (overlap)

DFA $M_1$ for $L_1$

DFA $M_2$