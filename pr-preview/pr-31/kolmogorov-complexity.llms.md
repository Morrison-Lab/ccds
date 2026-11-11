# Kolmogorov complexity

Code

Published

Last modified: 2026-11-11 00:40:31 (PST)

Shannon entropy measures the information in a random source: how many binary digits, on average, its outcomes need. Kolmogorov complexity measures the information in one string: the length of the shortest program that prints it. This page defines Kolmogorov complexity, shows that most strings cannot be compressed and that no program computes it, and shows that its expected value per symbol approaches the entropy. It builds on the [entropy](https://morrison-lab.github.io/ids/entropy.html) and [source coding](https://morrison-lab.github.io/ids/source-coding.html) pages in *Information Theory for Data Science*.

## 1 Shortest descriptions

> **NOTE:**
>
> **Definition 1 (Kolmogorov complexity)** Fix a universal computer $U$: a [Turing machine](models-of-computation.llms.md#def-turing-machine), or an interpreter for a programming language, that reads a program $p$, a finite string of binary digits, and either runs forever or halts and outputs a binary string $U(p)$. Assume no program on which $U$ halts is a proper prefix of another, as happens when $U$ reads its program one digit at a time and must itself decide where the program ends.
>
> The **Kolmogorov complexity** of a binary string $x$ is the length of a shortest program that outputs $x$:
>
> $${K\mathopen{}\left(x\right)\mathclose{}} \stackrel{\text{def}}{=}\min {\mathopen{}\left\{{\mathopen{}\left|p\right|\mathclose{}} : U(p) = x\right\}\mathclose{}},$$
>
> where ${\mathopen{}\left|p\right|\mathclose{}}$ is the number of digits in $p$. This is the definition of Cover and Thomas ([2006, sec. 14.2](#ref-cover2006elements)).

> **NOTE:**
>
> **Definition 2 (Conditional Kolmogorov complexity)** Let the universal computer $U$ of [Definition 1](#def-kolmogorov-complexity) also receive an integer $n$ as an input, so that it outputs $U(p, n)$ if it halts on program $p$. Assume that, for each fixed $n$, no program on which $U$ halts is a proper prefix of another. The **conditional Kolmogorov complexity** of a binary string $x$ given $n$ is
>
> $${K\mathopen{}\left(x \mid n\right)\mathclose{}} \stackrel{\text{def}}{=}\min {\mathopen{}\left\{{\mathopen{}\left|p\right|\mathclose{}} : U(p, n) = x\right\}\mathclose{}}.$$
>
> Cover and Thomas ([2006, sec. 14.2](#ref-cover2006elements)) define it the same way.

> **NOTE:**
>
> **Theorem 1 (The choice of computer changes** $K$ by at most a constant) Let $K_U$ and $K_A$ be the [Kolmogorov complexities](#def-kolmogorov-complexity) and [conditional Kolmogorov complexities](#def-conditional-kolmogorov-complexity) defined with universal computers $U$ and $A$. There is a constant $c_A$, which depends on $U$ and $A$ but not on $x$ or $n$, such that
>
> $$K_U\mathopen{}\left(x\right)\mathclose{} \le K_A\mathopen{}\left(x\right)\mathclose{} + c_A
> \quad \text{and} \quad
> K_U\mathopen{}\left(x \mid n\right)\mathclose{} \le K_A\mathopen{}\left(x \mid n\right)\mathclose{} + c_A$$
>
> for every binary string $x$ and every integer $n$.

> **NOTE:**
>
> *Proof*. Because $U$ is universal, some program $s_A$ makes $U$ simulate $A$: given $s_A$ followed by a program $p$ for $A$, $U$ outputs whatever $A$ outputs on $p$. So if $p$ is a shortest program for $x$ on $A$, then $s_A$ followed by $p$ is a program for $x$ on $U$, and $K_U\mathopen{}\left(x\right)\mathclose{} \le {\mathopen{}\left|s_A\right|\mathclose{}} + {\mathopen{}\left|p\right|\mathclose{}} = K_A\mathopen{}\left(x\right)\mathclose{} + {\mathopen{}\left|s_A\right|\mathclose{}}$. The same program $s_A$ also passes $n$ on to $A$, which gives the conditional bound in the same way. Take $c_A = {\mathopen{}\left|s_A\right|\mathclose{}}$. Cover and Thomas ([2006, sec. 14.2](#ref-cover2006elements)) give this argument in full.

> **NOTE:**
>
> *Remark 1* (Why the choice of computer hardly matters). Applying [Theorem 1](#thm-kolmogorov-invariance) with the roles of $U$ and $A$ swapped gives a constant $c'_A$ with $K_A\mathopen{}\left(x\right)\mathclose{} \le K_U\mathopen{}\left(x\right)\mathclose{} + c'_A$. Together, $\mathopen{}\left|K_U\mathopen{}\left(x\right)\mathclose{} - K_A\mathopen{}\left(x\right)\mathclose{}\right|\mathclose{} \le \max(c_A, c'_A)$ for every $x$, and in the same way $\mathopen{}\left|K_U\mathopen{}\left(x \mid n\right)\mathclose{} - K_A\mathopen{}\left(x \mid n\right)\mathclose{}\right|\mathclose{} \le \max(c_A, c'_A)$ for every $x$ and $n$. The constant can be large, so the complexity of one short string means little, but statements about how ${K\mathopen{}\left(x\right)\mathclose{}}$ grows with the length of $x$ do not depend on the computer. From here on, $U$ is fixed and dropped from the notation.

> **NOTE:**
>
> **Theorem 2 (A string is never much more complex than its length)** There is a constant $c$ such that every binary string $x$ of length $n$ has
>
> $${K\mathopen{}\left(x \mid n\right)\mathclose{}} \le n + c$$
>
> and, when $n \ge 1$,
>
> $${K\mathopen{}\left(x\right)\mathclose{}} \le n + 2 \log_2 n + c.$$

> **NOTE:**
>
> *Proof*. For the first bound, the program “print the next $n$ digits” followed by the $n$ digits of $x$ outputs $x$. Because $U$ is given $n$, the program needs no marker to show where it ends, and its fixed part has some length $c_1$. So ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \le n + c_1$.
>
> For the second bound, the program must also describe $n$ in a way that shows where the description ends. Write $n$ in binary, which takes $\mathopen{}\left\lfloor\log_2 n\right\rfloor\mathclose{} + 1$ digits, repeat each digit twice, and end with the pair $01$. This takes $2 \mathopen{}\left\lfloor\log_2 n\right\rfloor\mathclose{} + 4 \le 2 \log_2 n + 4$ digits. A program that reads this description of $n$ and then prints the next $n$ digits has length at most $n + 2 \log_2 n + 4 + c_2$ for a constant $c_2$. Take $c = \max(c_1, c_2 + 4)$. Cover and Thomas ([2006, sec. 14.2](#ref-cover2006elements)) prove both bounds.

> **NOTE:**
>
> **Example 1 (Simple strings)**  
>
> - For $n \ge 1$, the string $x$ of $n$ zeros has ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \le c$ for a constant $c$, since the program “print $n$ zeros” does not grow with $n$. Without $n$ given, the program must also describe $n$, and the proof of [Theorem 2](#thm-kolmogorov-upper-bound) gives ${K\mathopen{}\left(x\right)\mathclose{}} \le 2 \log_2 n + c'$.
>
> - The string $x = 0101\ldots01$ of length $n$, for even $n \ge 2$, also has ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \le c$, since the program “print $01$ until $n$ digits are printed” does not grow with $n$. Half its digits are ones, as expected of $n$ fair coin flips.
>
> - The string $x$ of the first $n$ binary digits of $\pi$ also has ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \le c$: a fixed program computes as many digits of $\pi$ as asked. The digits show no obvious pattern, yet they are as compressible as a string of zeros.

## 2 Incompressible strings

> **NOTE:**
>
> **Theorem 3 (Most strings are incompressible)** Let $n \ge 1$ and $0 \le k \le n$ be integers. Fewer than $2^{n-k}$ of the $2^n$ binary strings of length $n$ satisfy ${K\mathopen{}\left(x \mid n\right)\mathclose{}} < n - k$. In particular, at least one string of length $n$ has ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \ge n$.

> **NOTE:**
>
> *Proof*. There are $2^j$ binary strings of length $j$, so the number of programs of length less than $n - k$ is
>
> $$\sum_{j=0}^{n-k-1} 2^j = 2^{n-k} - 1,$$
>
> where the sum is empty, and equal to $0$, when $k = n$.
>
> Given $n$, each program outputs at most one string, so at most $2^{n-k} - 1$ strings $x$ have a program shorter than $n - k$. With $k = 0$, at most $2^n - 1$ of the $2^n$ strings of length $n$ have ${K\mathopen{}\left(x \mid n\right)\mathclose{}} < n$, which leaves at least one with ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \ge n$.

> **NOTE:**
>
> *Remark 2* (Incompressible strings look random). By [Theorem 3](#thm-incompressible-strings), if $x$ is drawn uniformly from the strings of length $n$, for example by $n$ fair coin flips, then ${\Pr\mathopen{}\left({K\mathopen{}\left(x \mid n\right)\mathclose{}} < n - k\right)\mathclose{}} < 2^{n-k} / 2^n = 2^{-k}$. So a string of $n$ fair coin flips can be compressed by more than $k$ digits with probability less than $2^{-k}$. A string with ${K\mathopen{}\left(x \mid n\right)\mathclose{}} \ge n$ has no description shorter than itself. Li and Vitányi ([2019, chap. 2](#ref-li2019introduction)) call a string random when it is incompressible in this sense, for a variant of $K$ whose programs need not be prefix-free. A proof can pick an incompressible string and show that some property must hold for it, or else the string would have a short description. Li and Vitányi ([2019, chap. 6](#ref-li2019introduction)) call this the *incompressibility method*.

## 3 Kolmogorov complexity is uncomputable

> **NOTE:**
>
> **Theorem 4 (Kolmogorov complexity cannot be computed)** No program takes a binary string $x$ as input and outputs ${K\mathopen{}\left(x\right)\mathclose{}}$.

> **NOTE:**
>
> *Proof*. Suppose a program $Q$ halts on every binary string $x$ and outputs ${K\mathopen{}\left(x\right)\mathclose{}}$. For each integer $m \ge 1$, consider the program $P_m$ that does the following: list binary strings in order of length, and in lexicographic order within each length, use $Q$ to compute ${K\mathopen{}\left(x\right)\mathclose{}}$ for each one, and output the first $x$ with ${K\mathopen{}\left(x\right)\mathclose{}} \ge m$. Such an $x$ exists, because fewer than $2^m$ programs are shorter than $m$, so $P_m$ halts, and its output $x$ has ${K\mathopen{}\left(x\right)\mathclose{}} \ge m$.
>
> The program $P_m$ consists of a fixed part, of some length $c$, and a description of $m$ that shows where it ends, which the proof of [Theorem 2](#thm-kolmogorov-upper-bound) writes in at most $2 \log_2 m + 4$ digits. So the output $x$ of $P_m$ also has ${K\mathopen{}\left(x\right)\mathclose{}} \le 2 \log_2 m + 4 + c$, and together
>
> $$m \le {K\mathopen{}\left(x\right)\mathclose{}} \le 2 \log_2 m + 4 + c.$$
>
> The left side grows faster than the right, so this fails once $m$ is large enough, and no such $Q$ exists. The argument formalizes the Berry paradox, “the smallest positive integer not definable in under sixty letters”, and Cover and Thomas ([2006, chap. 14](#ref-cover2006elements)) discuss why $K$ cannot be computed.

## 4 Kolmogorov complexity and entropy

> **NOTE:**
>
> **Theorem 5 (Expected Kolmogorov complexity per symbol approaches the entropy)** Let $X_1, X_2, \ldots$ be independent copies of a random variable $X$ that takes values in a finite set $\mathcal{X}$, each value written as a different binary string of one fixed length, and let ${H\mathopen{}\left(X\right)\mathclose{}}$ be the [entropy](https://morrison-lab.github.io/pds/expectation.html#def-entropy) of $X$ with base-2 logarithms. Write $X^n = (X_1, \ldots, X_n)$. There is a constant $c$, which depends on $\mathcal{X}$ and $U$ but not on $n$, such that, for every $n \ge 1$,
>
> $${H\mathopen{}\left(X\right)\mathclose{}}
> \le \frac{1}{n} {\mathbb{E}\mathopen{}\left[{K\mathopen{}\left(X^n \mid n\right)\mathclose{}}\right]\mathclose{}}
> \le {H\mathopen{}\left(X\right)\mathclose{}} + \frac{(\mathopen{}\left|\mathcal{X}\right|\mathclose{} - 1) \log_2 n}{n} + \frac{c}{n}.$$
>
> So $\frac{1}{n} {\mathbb{E}\mathopen{}\left[{K\mathopen{}\left(X^n \mid n\right)\mathclose{}}\right]\mathclose{}} \to {H\mathopen{}\left(X\right)\mathclose{}}$ as $n \to \infty$. This is Theorem 14.3.1 of Cover and Thomas ([2006](#ref-cover2006elements)).

> **NOTE:**
>
> *Proof*. For the lower bound, send each outcome $x^n$ of $X^n$ to a shortest program that outputs it given $n$. Different outcomes get different programs, because a program determines its output, and no halting program is a prefix of another, so this is a prefix code for $X^n$, which makes it [uniquely decodable](https://morrison-lab.github.io/ids/source-coding.html#def-uniquely-decodable). Its [expected length](https://morrison-lab.github.io/ids/source-coding.html#def-expected-length) is ${\mathbb{E}\mathopen{}\left[{K\mathopen{}\left(X^n \mid n\right)\mathclose{}}\right]\mathclose{}}$. If $X$ takes only one value with positive probability, then ${H\mathopen{}\left(X\right)\mathclose{}} = 0$ and the lower bound holds trivially. Otherwise, $X^n$ also takes at least two values with positive probability, and part 1 of the [source coding theorem](https://morrison-lab.github.io/ids/source-coding.html#thm-source-coding) gives ${\mathbb{E}\mathopen{}\left[{K\mathopen{}\left(X^n \mid n\right)\mathclose{}}\right]\mathclose{}} \ge {H\mathopen{}\left(X^n\right)\mathclose{}}$. Applying the [chain rule for entropy](https://morrison-lab.github.io/ids/entropy.html#thm-entropy-chain-rule) $n - 1$ times, and using that conditioning on independent variables does not change entropy, gives ${H\mathopen{}\left(X^n\right)\mathclose{}} = n {H\mathopen{}\left(X\right)\mathclose{}}$. Dividing by $n$ gives the lower bound.
>
> For the upper bound, Cover and Thomas ([2006, sec. 14.3](#ref-cover2006elements)) describe $x^n$ in two parts: first how many times each value in $\mathcal{X}$ occurs, which takes about $(\mathopen{}\left|\mathcal{X}\right|\mathclose{} - 1) \log_2 n$ digits, then which of the strings with those counts $x^n$ is, which takes about $n {H\mathopen{}\left(X\right)\mathclose{}}$ digits on average.

> **NOTE:**
>
> *Remark 3* (Entropy describes a distribution; Kolmogorov complexity describes a string). Entropy is the expected [surprisal](https://morrison-lab.github.io/ids/entropy.html#def-surprisal) of a random variable, so it is a property of a distribution and says nothing about any one outcome. Kolmogorov complexity is a property of one string and needs no distribution. [Theorem 5](#thm-kolmogorov-entropy) says that the two agree on average, per symbol, for long independent sequences. For a single string they can disagree: the string $0101\ldots01$ in [Example 1](#exm-kolmogorov-simple-strings) has as many ones as zeros, like a typical outcome of fair coin flips, whose entropy is 1 bit per flip, but its Kolmogorov complexity per digit, ${K\mathopen{}\left(x \mid n\right)\mathclose{}}/n \le c/n$, tends to 0.

## References

Cover, Thomas M., and Joy A. Thomas. 2006. *Elements of Information Theory*. 2nd ed. Wiley-Interscience. <https://doi.org/10.1002/047174882X>.

Li, Ming, and Paul Vitányi. 2019. *An Introduction to Kolmogorov Complexity and Its Applications*. 4th ed. Springer. <https://doi.org/10.1007/978-3-030-11298-1>.

Back to top
