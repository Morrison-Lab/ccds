# Computability

Code

Published

Last modified: 2026-11-11 01:02:38 (PST)

Some problems have no algorithm at all, however much time is allowed. No [Turing machine](models-of-computation.llms.md#def-turing-machine) [decides](models-of-computation.llms.md#def-tm-computes) whether a program [halts](models-of-computation.llms.md#def-turing-machine). The proof uses an argument called diagonalization, and the same argument shows that more time lets machines decide more problems. These results build on the [models of computation](models-of-computation.llms.md) page, which defines Turing machines, their [descriptions](models-of-computation.llms.md#def-tm-description) as strings, and the universal machine.

## 1 Decidable problems

> **NOTE:**
>
> **Definition 1 (Decidable problem)** A [decision problem](p-and-np.llms.md#def-decision-problem) $L$ is **decidable** if some [Turing machine](models-of-computation.llms.md#def-turing-machine) [decides](models-of-computation.llms.md#def-tm-computes) it: the machine [halts](models-of-computation.llms.md#def-turing-machine) on every input $x$, with [output](models-of-computation.llms.md#def-turing-machine) $1$ if $x \in L$ and $0$ if $x \notin L$. A problem that no Turing machine decides is *undecidable* ([Arora and Barak 2009, sec. 1.5](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 1 (Decidable problems from P and NP)** The palindromes are [decidable](#def-decidable), because the machine of [the palindrome example](models-of-computation.llms.md#exm-palindromes-linear-time) [decides](models-of-computation.llms.md#def-tm-computes) them. [INDSET](p-and-np.llms.md#def-indset) is decidable too, even though no [polynomial-time](p-and-np.llms.md#def-running-time) algorithm for it is known. It is in [NP](p-and-np.llms.md#def-class-np), whose definition gives a verifier and a certificate length $q({\mathopen{}\left|x\right|\mathclose{}})$. A machine can run the verifier on each of the $2^{q({\mathopen{}\left|x\right|\mathclose{}})}$ possible certificates in turn, and accept if any certificate passes. That search takes exponential time, but it always [halts](models-of-computation.llms.md#def-turing-machine). Decidability asks only whether a machine that always halts exists, not how long it takes.

## 2 An undecidable problem

> **NOTE:**
>
> **Definition 2 (The diagonal problem)** The **diagonal problem** UC is the [decision problem](p-and-np.llms.md#def-decision-problem) whose yes inputs are the strings $w$ such that the machine $M_w$ that $w$ [describes](models-of-computation.llms.md#def-tm-description), run on its own [description](models-of-computation.llms.md#def-tm-description) $w$, does not [halt](models-of-computation.llms.md#def-turing-machine) with [output](models-of-computation.llms.md#def-turing-machine) $1$:
>
> $$\text{UC} \stackrel{\text{def}}{=}{\mathopen{}\left\{w \in {\mathopen{}\left\{0, 1\right\}\mathclose{}}^* : M_w(w) \ne 1\right\}\mathclose{}},$$
>
> where $M_w(w) \ne 1$ includes the case that $M_w$ never halts on $w$ ([Arora and Barak 2009, sec. 1.5](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 2 (Machines in and out of the diagonal problem)** If $w$ [describes](models-of-computation.llms.md#def-tm-description) a machine that [outputs](models-of-computation.llms.md#def-turing-machine) $1$ on every input, then $M_w(w) = 1$, so $w$ is not in [UC](#def-diagonal-problem). If $w$ describes a machine that outputs $0$ on every input, or one that runs forever on every input, then $w$ is in UC. If $w$ describes the palindrome machine of [the palindrome example](models-of-computation.llms.md#exm-palindrome-machine), then $w$ is in UC exactly when $w$ is not itself a palindrome.

> **NOTE:**
>
> **Theorem 1 (The diagonal problem is undecidable)** The [diagonal problem](#def-diagonal-problem) UC is [undecidable](#def-decidable) ([Arora and Barak 2009, sec. 1.5](#ref-arora2009computational)).

> **NOTE:**
>
> *Proof*. Suppose a [Turing machine](models-of-computation.llms.md#def-turing-machine) $M$ [decides](models-of-computation.llms.md#def-tm-computes) UC, and let $w$ be a [description](models-of-computation.llms.md#def-tm-description) of $M$, so that $M_w = M$. Then $M$ [halts](models-of-computation.llms.md#def-turing-machine) on $w$ with [output](models-of-computation.llms.md#def-turing-machine) $1$ or $0$.
>
> - If $M(w) = 1$, then $w \in \text{UC}$, because $M$ decides UC. By [Definition 2](#def-diagonal-problem), $w \in \text{UC}$ means $M_w(w) \ne 1$, that is, $M(w) \ne 1$, a contradiction.
> - If $M(w) = 0$, then $w \notin \text{UC}$, because $M$ decides UC. By [Definition 2](#def-diagonal-problem), $w \notin \text{UC}$ means $M_w(w) = 1$, that is, $M(w) = 1$, a contradiction.
>
> Both cases are impossible, so no Turing machine decides UC.

> **NOTE:**
>
> *Remark 1* (The diagonal argument). Picture an infinite table with one row for each binary string $w$, one column for each binary string $x$, and the entry in row $w$ and column $x$ equal to $M_w(x)$, or to a mark for “runs forever”. Each row lists everything one machine does. UC is built from the diagonal entries $M_w(w)$, and it disagrees with row $w$ in column $w$ for every $w$, so no row [decides](models-of-computation.llms.md#def-tm-computes) it. Arora and Barak ([2009, chap. 3](#ref-arora2009computational)) use the same idea to prove the hierarchy theorems.

## 3 The halting problem

> **NOTE:**
>
> **Definition 3 (The halting problem)** The **halting problem** HALT is the [decision problem](p-and-np.llms.md#def-decision-problem)
>
> $$\text{HALT} \stackrel{\text{def}}{=}{\mathopen{}\left\{\mathopen{}\left\langle w, x\right\rangle\mathclose{} : M_w \text{ halts on input } x\right\}\mathclose{}},$$
>
> where $\mathopen{}\left\langle w, x\right\rangle\mathclose{}$ is the [pair encoding](models-of-computation.llms.md#def-pair-encoding) of $w$ and $x$, and $M_w$ is the machine that $w$ [describes](models-of-computation.llms.md#def-tm-description) ([Arora and Barak 2009, sec. 1.5](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 3 (Pairs in and out of the halting problem)** Suppose $w$ [describes](models-of-computation.llms.md#def-tm-description) a machine that [halts](models-of-computation.llms.md#def-turing-machine) at once if the first symbol of its input is $0$, and otherwise moves its input head right forever. Then $\mathopen{}\left\langle w, 0\right\rangle\mathclose{}$ and $\mathopen{}\left\langle w, 011\right\rangle\mathclose{}$ are in [HALT](#def-halting-problem), and $\mathopen{}\left\langle w, 1\right\rangle\mathclose{}$ is not. Neither is $\mathopen{}\left\langle w, x\right\rangle\mathclose{}$ for the empty string $x$, because the first symbol the machine reads after $\triangleright$ is then a blank, not $0$.

> **NOTE:**
>
> **Theorem 2 (The halting problem is undecidable)** The [halting problem](#def-halting-problem) is [undecidable](#def-decidable) ([Turing 1937](#ref-turing1937computable); [Arora and Barak 2009, sec. 1.5](#ref-arora2009computational)).

> **NOTE:**
>
> *Proof*. Suppose a [Turing machine](models-of-computation.llms.md#def-turing-machine) $H$ [decides](models-of-computation.llms.md#def-tm-computes) [HALT](#def-halting-problem). Build a machine $D$ that, on input $w$, does the following:
>
> 1.  Run $H$ on $\mathopen{}\left\langle w, w\right\rangle\mathclose{}$.
> 2.  If $H$ [outputs](models-of-computation.llms.md#def-turing-machine) $0$, then $M_w$ does not halt on $w$, so $M_w(w) \ne 1$ and $w \in \text{UC}$: output $1$.
> 3.  If $H$ outputs $1$, then $M_w$ halts on $w$. Run the [universal machine](models-of-computation.llms.md#thm-universal-tm) $U$ on $\mathopen{}\left\langle w, w\right\rangle\mathclose{}$, with $w$ as the [description](models-of-computation.llms.md#def-tm-description) and as the input, which halts with output $M_w(w)$. Output $0$ if $M_w(w) = 1$, and $1$ otherwise.
>
> $H$ halts on every input, and step 3 runs $U$ only when $M_w$ halts on $w$, so $D$ halts on every input. Its output is $1$ exactly when $M_w(w) \ne 1$, so $D$ decides the [diagonal problem](#def-diagonal-problem) UC. This contradicts [Theorem 1](#thm-uc-undecidable), so no Turing machine decides HALT.

> **NOTE:**
>
> *Remark 2* (Undecidability in practice). [Turing machines](models-of-computation.llms.md#def-turing-machine) can simulate programs in ordinary programming languages ([the Church-Turing thesis](models-of-computation.llms.md#rem-church-turing)), so [Theorem 2](#thm-halting-undecidable) says that no program can check, for every program and input, whether that program finishes. A job scheduler that runs users’ model-fitting code cannot predict which jobs will loop forever, so it stops jobs with a time limit instead. Tools that always finish and check code for infinite loops exist. If such a tool never answered “unknown”, never missed a loop, and never flagged a program that does [halt](models-of-computation.llms.md#def-turing-machine), then, since an ordinary language can run a Turing machine simulator, the tool applied to a simulator running $M_w$ on $x$ would decide [HALT](#def-halting-problem). So each tool makes one of those three errors on some program. The proof that [Kolmogorov complexity](kolmogorov-complexity.llms.md#thm-kolmogorov-uncomputable) cannot be computed is a second example of an uncomputable function, proved by a different argument.

## 4 The time hierarchy theorem

> **NOTE:**
>
> **Definition 4 (Time complexity class)** Let $T$ be a function from the nonnegative integers to the nonnegative real numbers. The class $\text{DTIME}(T(n))$ is the set of [decision problems](p-and-np.llms.md#def-decision-problem) $L$ such that some [Turing machine](models-of-computation.llms.md#def-turing-machine) [decides](models-of-computation.llms.md#def-tm-computes) $L$ and [runs in time](p-and-np.llms.md#def-running-time) $T'(n)$ for some function $T'$ with $T'(n) = {O\mathopen{}\left(T(n)\right)\mathclose{}}$ ([big-O notation](asymptotic-notation.llms.md#def-big-o)) (adapted from [Arora and Barak 2009, sec. 1.6](#ref-arora2009computational)).

> **NOTE:**
>
> **Example 4 (Palindromes and the class P)** The [palindrome machine](models-of-computation.llms.md#exm-palindromes-linear-time) [runs in time](p-and-np.llms.md#def-running-time) $3n + 4$, which is at most $7n$ for every $n \ge 1$ and so is ${O\mathopen{}\left(n\right)\mathclose{}}$, so PAL is in $\text{DTIME}(n)$ ([time complexity class](#def-dtime)). [The class P](p-and-np.llms.md#def-class-p) is the union of $\text{DTIME}(n^c)$ over all constants $c \ge 1$.

> **NOTE:**
>
> **Theorem 3 (More time decides more problems)** The [time complexity class](#def-dtime) $\text{DTIME}(n)$ is a proper subset of $\text{DTIME}(n^{1.5})$ ([Arora and Barak 2009, sec. 3.1](#ref-arora2009computational)).

> **NOTE:**
>
> *Proof*. Every problem in $\text{DTIME}(n)$ is in $\text{DTIME}(n^{1.5})$, because $n = {O\mathopen{}\left(n^{1.5}\right)\mathclose{}}$. For the other half, let $U$ be the [universal machine](models-of-computation.llms.md#thm-universal-tm), and build a machine $D$ that, on an input $x$ of length $n$, does the following:
>
> 1.  Copy the [pair encoding](models-of-computation.llms.md#def-pair-encoding) $\mathopen{}\left\langle x, x\right\rangle\mathclose{}$ onto a work tape.
> 2.  Compute the number $t = \mathopen{}\left\lfloor n^{1.4}\right\rfloor\mathclose{}$ in binary on another work tape.
> 3.  Run $U$ on $\mathopen{}\left\langle x, x\right\rangle\mathclose{}$, which runs the machine that $x$ [describes](models-of-computation.llms.md#def-tm-description) on input $x$, and subtract $1$ from $t$ after each step of $U$. Stop $U$ when $t$ reaches $0$.
> 4.  If $U$ halted within those steps with [output](models-of-computation.llms.md#def-turing-machine) $b \in {\mathopen{}\left\{0, 1\right\}\mathclose{}}$, output $1 - b$. Otherwise output $0$.
>
> Steps 1 and 2 take ${O\mathopen{}\left(n \log_2 n\right)\mathclose{}}$ steps: $D$ writes $\mathopen{}\left\langle x, x\right\rangle\mathclose{}$, counts the symbols of $x$ in binary, then does arithmetic on numbers of ${O\mathopen{}\left(\log_2 n\right)\mathclose{}}$ digits. Step 3 runs $U$ for at most $n^{1.4}$ steps, and each subtraction takes ${O\mathopen{}\left(\log_2 n\right)\mathclose{}}$ steps, so step 3 takes ${O\mathopen{}\left(n^{1.4} \log_2 n\right)\mathclose{}}$ steps, which is ${O\mathopen{}\left(n^{1.5}\right)\mathclose{}}$ because $\log_2 n = {O\mathopen{}\left(n^{0.1}\right)\mathclose{}}$. So $D$ [halts](models-of-computation.llms.md#def-turing-machine) on every input, and the problem $L$ that $D$ [decides](models-of-computation.llms.md#def-tm-computes) is in $\text{DTIME}(n^{1.5})$.
>
> Suppose $L$ is also in $\text{DTIME}(n)$. Then some [Turing machine](models-of-computation.llms.md#def-turing-machine) $M$ decides $L$, and there are a constant $c \ge 1$ and an integer $n_0$ such that $M$ halts within $c \, n$ steps on every input of length $n \ge n_0$. Let $x$ be an input of length $n \ge n_0$, and let $w$ be any [description](models-of-computation.llms.md#def-tm-description) of $M$. By [the universal machine theorem](models-of-computation.llms.md#thm-universal-tm), $U$ on $\mathopen{}\left\langle w, x\right\rangle\mathclose{}$ halts with output $M(x)$ within $C \mathopen{}\left({\mathopen{}\left|w\right|\mathclose{}} + c \, n \log_2 (c \, n + 1)\right)\mathclose{}$ steps, where $C$ depends on $M$ but not on $w$ or $x$. When $w = x$, so that ${\mathopen{}\left|w\right|\mathclose{}} = n$, that bound is ${O\mathopen{}\left(n \log_2 n\right)\mathclose{}}$, so there is an $n_1 \ge n_0$ such that the bound is less than $\mathopen{}\left\lfloor n^{1.4}\right\rfloor\mathclose{}$ for every $n \ge n_1$. $M$ has infinitely many descriptions, so it has a description $x$ of length at least $n_1$. On that input, $U$ finishes within the $\mathopen{}\left\lfloor n^{1.4}\right\rfloor\mathclose{}$ steps that $D$ allows, with output $b = M(x)$, so $D(x) = 1 - M(x) \ne M(x)$. But $M$ and $D$ both decide $L$, so $M(x) = D(x)$, a contradiction. So $L$ is not in $\text{DTIME}(n)$.

> **NOTE:**
>
> **Theorem 4 (The time hierarchy theorem)** Let $f$ and $g$ be [time-constructible functions](models-of-computation.llms.md#def-time-constructible) with $f(n) \log_2 f(n) = o(g(n))$ ([little-o notation](asymptotic-notation.llms.md#def-little-o)). Then the [time complexity class](#def-dtime) $\text{DTIME}(f(n))$ is a proper subset of $\text{DTIME}(g(n))$ ([Arora and Barak 2009, sec. 3.1](#ref-arora2009computational)).

> **NOTE:**
>
> *Remark 3* (Proof and consequences of the time hierarchy theorem). A refinement of the proof of [Theorem 3](#thm-time-hierarchy) proves [Theorem 4](#thm-time-hierarchy-general). It uses the same diagonal machine, but with a clock that costs a constant number of steps per simulated step on average, in place of the ${O\mathopen{}\left(\log_2 n\right)\mathclose{}}$ steps per subtraction in the proof of [Theorem 3](#thm-time-hierarchy). That proof could afford the extra $\log_2 n$ factor only because $n^{1.4}$ and $n^{1.5}$ differ by a power of $n$. The $\log_2 f(n)$ factor in the hypothesis is the overhead of the universal machine in [the universal machine theorem](models-of-computation.llms.md#thm-universal-tm). Since $n^c \log_2 n^c = o(n^{c + 1})$, $\text{DTIME}(n^c)$ is a proper subset of $\text{DTIME}(n^{c + 1})$ for every integer $c \ge 1$, so [the class P](p-and-np.llms.md#def-class-p) is not equal to $\text{DTIME}(n^c)$ for any single constant $c$.

## References

Arora, Sanjeev, and Boaz Barak. 2009. *Computational Complexity: A Modern Approach*. Cambridge University Press. <https://doi.org/10.1017/CBO9780511804090>.

Turing, A. M. 1937. “On Computable Numbers, with an Application to the Entscheidungsproblem.” *Proceedings of the London Mathematical Society* s2-42 (1): 230–65. <https://doi.org/10.1112/plms/s2-42.1.230>.

Back to top
