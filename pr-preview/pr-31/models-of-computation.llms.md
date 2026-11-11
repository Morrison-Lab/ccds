# Models of computation

Code

Published

Last modified: 2026-11-11 00:40:31 (PST)

To say that a problem needs a certain number of steps, we must first fix what a step is. This page defines the [Turing machine](#def-turing-machine), the model of computation that complexity theory uses, and shows why the choice of model hardly matters: the variants of the model, and ordinary computers, simulate each other with at most a polynomial increase in [running time](p-and-np.llms.md#def-running-time). It ends with the universal machine, one Turing machine that can run every other. The [asymptotic notation](asymptotic-notation.llms.md) page defines the ${O\mathopen{}\left(\cdot\right)\mathclose{}}$ notation used here.

## 1 Turing machines

> **NOTE:**
>
> **Definition 1 (Turing machine)** A **Turing machine** with $k \ge 2$ tapes consists of:
>
> - $k$ tapes, each a sequence of cells that starts at a leftmost cell and goes on forever to the right, with one head per tape that reads and writes one cell at a time. The first tape is the *input tape*, which the machine can read but not write; the last tape is the *output tape*; any others are *work tapes*.
> - A finite *alphabet* $A$ of the symbols a cell can hold. It includes the binary digits $0$ and $1$, a blank symbol $\square$, and a start symbol $\triangleright$.
> - A finite set $Q$ of *states*, which includes a start state $q_{\text{start}}$ and a halting state $q_{\text{halt}}$.
> - A *transition function*, a map from $Q \times A^k$ to $Q \times A^{k - 1} \times {\mathopen{}\left\{\text{L}, \text{S}, \text{R}\right\}\mathclose{}}^k$.
>
> The machine starts in state $q_{\text{start}}$ with every head on its tape’s leftmost cell. Each tape holds $\triangleright$ in its leftmost cell and blanks after it, except that the input tape holds the input $x \in {\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$ right after the $\triangleright$. In one *step*, the machine reads its state and the $k$ symbols under its heads, and the transition function gives a new state, a symbol to write on each tape but the input tape, and a move for each head: one cell left (L), stay (S), or one cell right (R). A head told to move left from the leftmost cell stays where it is. The machine *halts* when it enters $q_{\text{halt}}$, and its *output* is then the string of binary digits on the output tape, read from the cell after $\triangleright$ up to the first symbol that is not $0$ or $1$ ([Arora and Barak 2009, sec. 1.2](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 1 (A machine that checks for palindromes)** A binary string is a *palindrome* if it reads the same backward as forward, like $0110$ or $10101$. A [Turing machine](#def-turing-machine) with three tapes (input, one work tape, and [output](#def-turing-machine)) can check whether its input $x$ is a palindrome in three phases ([Arora and Barak 2009](#ref-arora2009computational), example 1.1):
>
> 1.  *Copy*: move the input head and the work head right together, writing each symbol of $x$ onto the work tape, until the input head reaches the blank after $x$. The first step also moves the output head right, off $\triangleright$.
> 2.  *Rewind*: move the input head left until it reads $\triangleright$, then one cell right, onto the first symbol of $x$. Move the work head one cell left, onto the last symbol of the copy.
> 3.  *Compare*: move the input head right and the work head left together. If the two heads ever read different symbols, write $0$ on the output tape and [halt](#def-turing-machine). If the input head reaches the blank after $x$ without a mismatch, write $1$ and halt.
>
> On the input $0110$, the compare phase reads the pairs $(0, 0)$, $(1, 1)$, $(1, 1)$, and $(0, 0)$, finds no mismatch, and outputs $1$. On $0111$ it reads $(0, 1)$ first and outputs $0$. Each phase is a few states of the machine, and each state of a phase checks which symbols the heads read to decide whether to stay in the phase or move to the next.

> **NOTE:**
>
> **Definition 2 (Computing a function)** Let $f$ be a function from ${\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$ to ${\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$. A [Turing machine](#def-turing-machine) $M$ **computes** $f$ if $M$ [halts](#def-turing-machine) on every input $x \in {\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$ with [output](#def-turing-machine) $f(x)$. Write $M(x)$ for the output of $M$ on input $x$. A Turing machine [decides](p-and-np.llms.md#def-decision-problem) a [decision problem](p-and-np.llms.md#def-decision-problem) $L$ exactly when it computes the function that is $1$ on $L$ and $0$ off it ([Arora and Barak 2009, sec. 1.2](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 2 (Palindromes in linear time)** Let PAL be the [decision problem](p-and-np.llms.md#def-decision-problem) whose yes inputs are the palindromes. The machine of [Example 1](#exm-palindrome-machine) [computes](#def-tm-computes) the function that is $1$ on palindromes and $0$ on other strings, so it [decides](#def-tm-computes) PAL. On an input of length $n$, the copy phase takes $n + 1$ steps, the rewind phase at most $n + 2$, and the compare phase at most $n + 1$, so it [runs in time](p-and-np.llms.md#def-running-time) $3n + 4$, which is ${O\mathopen{}\left(n\right)\mathclose{}}$, and PAL is in [P](p-and-np.llms.md#def-class-p).

> **NOTE:**
>
> *Remark 1* (Steps and running time). The [running time](p-and-np.llms.md#def-running-time) of a [Turing machine](#def-turing-machine) counts the steps of [Definition 1](#def-turing-machine), one application of the transition function each. One step moves each head by at most one cell, so reaching a cell $m$ cells away takes at least $m$ steps. An ordinary computer can jump to any memory address in one operation, so a Turing machine can need more steps than an ordinary program for the same task. [Theorem 1](#thm-tm-robustness) and [Remark 3](#rem-church-turing) explain why that gap does not change which problems have [polynomial-time](p-and-np.llms.md#def-running-time) algorithms.

## 2 Robustness of the model

> **NOTE:**
>
> **Definition 3 (Time-constructible function)** A function $T$ from the nonnegative integers to the nonnegative integers is **time-constructible** if $T(n) \ge n$ for every $n$ and some [Turing machine](#def-turing-machine) [computes](#def-tm-computes), in time $T(n)$, the function that maps each $x \in {\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$ to the binary representation of $T({\mathopen{}\left|x\right|\mathclose{}})$ ([Arora and Barak 2009, sec. 1.3](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 3 (Common time-constructible functions)** Four common examples, $n$, $n {\mathopen{}\left\lceil\log_2 (n + 1)\right\rceil\mathclose{}}$, $n^2$, and $2^n$, are all [time-constructible](#def-time-constructible) ([Arora and Barak 2009, sec. 1.3](#ref-arora2009computational)). A machine that is allowed $T(n)$ steps on inputs of length $n$ can, when $T$ is time-constructible, first write $T(n)$ on a work tape and then use it as a clock that stops a computation after $T(n)$ of its steps. The functions that bound [running times](p-and-np.llms.md#def-running-time) in these notes are all of this kind. Time-constructibility rules out functions such as ${\mathopen{}\left\lceil\log_2 (n + 1)\right\rceil\mathclose{}}$, which is smaller than $n$ once $n \ge 3$, so a machine cannot even read its whole input in that many steps.

> **NOTE:**
>
> **Theorem 1 (The details of the model change running time only polynomially)** Let $f$ be a function from ${\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$ to ${\mathopen{}\left\{0, 1\right\}\mathclose{}}^*$, and let $T$ be [time-constructible](#def-time-constructible). Suppose a [Turing machine](#def-turing-machine) $M$ with $k$ tapes and an alphabet of $s$ symbols [computes](#def-tm-computes) $f$ in time $T(n)$. Then:
>
> 1.  A Turing machine with $k$ tapes and the alphabet ${\mathopen{}\left\{0, 1, \square, \triangleright\right\}\mathclose{}}$ computes $f$ in time ${O\mathopen{}\left(T(n) \log_2 s\right)\mathclose{}}$.
> 2.  A single-tape machine computes $f$ in time ${O\mathopen{}\left(T(n)^2\right)\mathclose{}}$.
>
> A *single-tape machine* is the same as a Turing machine, except that it has one tape, which starts with $\triangleright$ followed by the input, which it can read and write, and which holds the output when it [halts](#def-turing-machine). The constant in part 2 depends on $k$, and neither constant depends on $f$, $T$, or $n$ ([Arora and Barak 2009, sec. 1.3](#ref-arora2009computational)).

> **NOTE:**
>
> *Remark 2* (Simulations behind [Theorem 1](#thm-tm-robustness)). Arora and Barak ([2009, sec. 1.3](#ref-arora2009computational)) prove both parts by simulation. For part 1, the new machine writes each symbol of $M$’s alphabet as a block of about $\log_2 s$ binary digits, and it carries out one step of $M$ by reading and rewriting one block on each tape. For part 2, the single tape holds the contents of all $k$ tapes interleaved, with a mark at each head’s position. To carry out one step of $M$, the machine sweeps along the used part of the tape to read the marked symbols, and sweeps back to update them. After $t$ steps, $M$ has used at most $\max(n, t) + 1$ cells of each tape, which is ${O\mathopen{}\left(T(n)\right)\mathclose{}}$ because $T(n) \ge n$, so each sweep takes ${O\mathopen{}\left(T(n)\right)\mathclose{}}$ steps, and the $T(n)$ steps of $M$ take ${O\mathopen{}\left(T(n)^2\right)\mathclose{}}$ in all.

> **NOTE:**
>
> *Remark 3* (The Church-Turing thesis). Every model of computation that has been proposed and that can be built, from programming languages to random-access machines, can be simulated by a [Turing machine](#def-turing-machine). The *Church-Turing thesis* claims that every function a physical device can compute, a Turing machine can compute. It is a claim about the physical world, not a theorem. A stronger version, which quantum computers may refute, says the simulation costs at most a polynomial increase in [running time](p-and-np.llms.md#def-running-time) ([Arora and Barak 2009, sec. 1.6](#ref-arora2009computational)). Under that version, [the class P](p-and-np.llms.md#def-class-p) is the same whichever reasonable model it is defined with, as [Theorem 1](#thm-tm-robustness) shows for the variations of the Turing machine itself.

## 3 The universal machine

> **NOTE:**
>
> **Definition 4 (Machines as strings)** A [Turing machine](#def-turing-machine) is a finite object: its states, its alphabet, and the table of its transition function. So it can be written as a binary string, its **description**. Fix one such encoding, with two conventions ([Arora and Barak 2009, sec. 1.4](#ref-arora2009computational)):
>
> - Every binary string $w$ describes some Turing machine, written $M_w$. A string that is not a valid encoding describes one fixed machine.
> - Every Turing machine has infinitely many descriptions, because the encoding allows padding: if $w$ describes $M$, so do longer strings that extend $w$ with symbols the decoder ignores.

> **NOTE:**
>
> **Example 4 (A program file is a description)** A Python script saved to disk is a sequence of bytes, so it is a binary string, and the Python interpreter reads that string as a program, much as [Definition 4](#def-tm-description) reads a string as a [Turing machine](#def-turing-machine). Appending a comment line to the script gives a longer string that [describes](#def-tm-description) the same program, which is the padding convention of [Definition 4](#def-tm-description). A file that is not valid Python makes the interpreter stop with a syntax error, which plays the role of the fixed machine that invalid strings describe, for instance one that [halts](#def-turing-machine) at once.

> **NOTE:**
>
> **Definition 5 (Pair encoding)** Let $w$ and $x$ be binary strings. Their **pair encoding** $\mathopen{}\left\langle w, x\right\rangle\mathclose{}$ is the binary string made of each digit of $w$ written twice, then $01$, then $x$. A machine can recover $w$ and $x$ from $\mathopen{}\left\langle w, x\right\rangle\mathclose{}$ by reading two digits at a time until it reads $01$. Arora and Barak ([2009, sec. 0.1](#ref-arora2009computational)) use a variant that also doubles the digits of $x$.

> **NOTE:**
>
> **Example 5 (Encoding the pair** $10$ and $0$) The [pair encoding](#def-pair-encoding) of $w = 10$ and $x = 0$ is
>
> $$\mathopen{}\left\langle 10, 0\right\rangle\mathclose{} = 1100010.$$
>
> It consists of $11$ and $00$ for the two digits of $w$, then $01$, then the single digit of $x$. Reading two digits at a time gives $11$, $00$, and then $01$, which marks the end of $w$.

> **NOTE:**
>
> **Theorem 2 (A universal Turing machine)** There is a [Turing machine](#def-turing-machine) $U$ that runs any other machine from its [description](#def-tm-description). Given the [pair encoding](#def-pair-encoding) $\mathopen{}\left\langle w, x\right\rangle\mathclose{}$ of binary strings $w$ and $x$ as input, $U$ [outputs](#def-turing-machine) $M_w(x)$ whenever $M_w$ [halts](#def-turing-machine) on $x$, and runs forever when $M_w$ does not. If $M_w$ halts on $x$ within $T$ steps, then $U$ halts within $C \, T \log_2 T$ steps, where $C$ depends on $M_w$ but not on $x$ ([Arora and Barak 2009, sec. 1.4](#ref-arora2009computational)).

> **NOTE:**
>
> *Remark 4* (Interpreters are universal machines). A Python interpreter is a universal machine in the sense of [Theorem 2](#thm-universal-tm): one fixed program that runs any other program it is given as input. Arora and Barak ([2009, sec. 1.4](#ref-arora2009computational)) prove a version of [Theorem 2](#thm-universal-tm) with $C \, T^2$ steps in place of $C \, T \log_2 T$, by an argument like that of [Remark 2](#rem-tm-robustness-proof), and they prove the stated bound in section 1.7. The [Kolmogorov complexity](kolmogorov-complexity.llms.md#def-kolmogorov-complexity) of a string is defined with a universal machine that, unlike the one of [Theorem 2](#thm-universal-tm), reads a single program, and [halts](#def-turing-machine) only on programs none of which is a prefix of another.

## References

Arora, Sanjeev, and Boaz Barak. 2009. *Computational Complexity: A Modern Approach*. Cambridge University Press. <https://doi.org/10.1017/CBO9780511804090>.

Back to top
