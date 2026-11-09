# Reductions and NP-completeness

Code

Published

Last modified: 2026-11-09 08:56:57 (PST)

A reduction turns every question about one problem into a question about another, so that a fast algorithm for the second gives one for the first. Reductions compare the difficulty of the problems in [NP](p-and-np.llms.md#def-class-np), and single out the hardest of them.

## 1 Polynomial-time reductions

> **NOTE:**
>
> **Definition 1 (Polynomial-time reduction)** A [decision problem](p-and-np.llms.md#def-decision-problem) $L$ **reduces in polynomial time** to a decision problem $L'$, written $L \le_p L'$, if there is a function $f \colon \{0, 1\}^* \to \{0, 1\}^*$, computable in [polynomial time](p-and-np.llms.md#def-running-time), such that for every $x \in \{0, 1\}^*$,
>
> $$x \in L \quad \text{if and only if} \quad f(x) \in L'$$
>
> ([Arora and Barak 2009, sec. 2.2](#ref-arora2009computational)).

> **NOTE:**
>
> **Theorem 1 (Reductions carry polynomial-time algorithms back)** If $L \le_p L'$ ([Definition 1](#def-reduction)) and $L'$ is in [P](p-and-np.llms.md#def-class-p), then $L$ is in P.

> **NOTE:**
>
> *Proof*. Let $f$ be the reduction. A big-O bound holds from some $n_0$ on, and raising its constant to cover the finitely many lengths below $n_0$ turns it into a bound for every length. So there are constants $a, b \ge 1$ and $c, d \ge 0$ such that, on inputs of every length $n \ge 0$, computing $f$ takes at most $a (n + 1)^{c}$ steps, and some algorithm decides $L'$ within $b (n + 1)^{d}$ steps.
>
> On input $x$ of length $n$, compute $f(x)$ and then decide whether $f(x) \in L'$. The answer is correct because $x \in L$ exactly when $f(x) \in L'$.
>
> Each step writes at most some fixed number $k$ of output bits, so $|f(x)| \le k a (n + 1)^{c}$. Since $(n + 1)^{c} \ge 1$,
>
> $$\begin{aligned}
> |f(x)| + 1
> &\le k a (n + 1)^{c} + 1 \\
> &\le (k a + 1) (n + 1)^{c}.
> \end{aligned}$$
>
> The two stages together take at most
>
> $$\begin{aligned}
> a (n + 1)^{c} + b \mathopen{}\left(|f(x)| + 1\right)\mathclose{}^{d}
> &\le a (n + 1)^{c} + b \mathopen{}\left[(k a + 1) (n + 1)^{c}\right]\mathclose{}^{d} \\
> &= a (n + 1)^{c} + b (k a + 1)^{d} (n + 1)^{cd}
> \end{aligned}$$
>
> steps, which is $O(n^{\max(c, cd)})$, a polynomial bound.

> **NOTE:**
>
> **Theorem 2 (Reductions compose)** If $L \le_p L'$ and $L' \le_p L''$ ([Definition 1](#def-reduction)), then $L \le_p L''$.

> **NOTE:**
>
> *Proof*. Let $f$ reduce $L$ to $L'$ and $g$ reduce $L'$ to $L''$. For every $x$, $x \in L$ exactly when $f(x) \in L'$, which is exactly when $g(f(x)) \in L''$. Computing $f(x)$ and then $g$ of the result takes polynomial time, by the same count as in the proof of [Theorem 1](#thm-reduction-preserves-p), with the algorithm for $g$ in place of the algorithm for $L'$. So $x \mapsto g(f(x))$ reduces $L$ to $L''$.

## 2 NP-completeness

> **NOTE:**
>
> **Definition 2 (NP-hard and NP-complete)** A decision problem $L'$ is **NP-hard** if $L \le_p L'$ ([Definition 1](#def-reduction)) for every decision problem $L$ in [NP](p-and-np.llms.md#def-class-np). It is **NP-complete** if it is NP-hard and also in NP ([Arora and Barak 2009, sec. 2.2](#ref-arora2009computational)).

> **NOTE:**
>
> **Theorem 3 (One NP-hard problem in P puts all of NP in P)** If any NP-hard problem ([Definition 2](#def-np-complete)) is in [P](p-and-np.llms.md#def-class-p), then every problem in NP is in P.

> **NOTE:**
>
> *Proof*. Let $L'$ be NP-hard and in P, and let $L$ be any problem in NP. Then $L \le_p L'$ by [Definition 2](#def-np-complete), so $L$ is in P by [Theorem 1](#thm-reduction-preserves-p).

> **NOTE:**
>
> **Theorem 4 (New NP-complete problems from old)** If $L$ is NP-complete ([Definition 2](#def-np-complete)), $L \le_p L'$ ([Definition 1](#def-reduction)), and $L'$ is in [NP](p-and-np.llms.md#def-class-np), then $L'$ is NP-complete.

> **NOTE:**
>
> *Proof*. $L'$ is in NP by assumption. For any $L_0$ in NP, $L_0 \le_p L$ because $L$ is NP-hard, so $L_0 \le_p L'$ by [Theorem 2](#thm-reduction-transitive). So $L'$ is NP-hard, and hence NP-complete.

> **NOTE:**
>
> **Definition 3 (The satisfiability problem SAT)** The decision problem **SAT** asks: given a Boolean formula built from variables with AND, OR and NOT, is there an assignment of true and false to its variables that makes the formula true?

> **NOTE:**
>
> **Theorem 5 (Cook–Levin theorem)** [SAT](#def-sat) is NP-complete ([Definition 2](#def-np-complete)).

> **NOTE:**
>
> **Theorem 6 (Independent set is NP-complete)** [INDSET](p-and-np.llms.md#def-indset) is NP-complete ([Definition 2](#def-np-complete)) ([Arora and Barak 2009, sec. 2.4](#ref-arora2009computational)).

> **NOTE:**
>
> *Remark 1* (Where NP-completeness comes from). Stephen Cook and Leonid Levin proved [Theorem 5](#thm-cook-levin) independently in the early 1970s; Arora and Barak ([2009, sec. 2.3](#ref-arora2009computational)) give a proof. Once one problem is known to be NP-complete, [Theorem 4](#thm-np-complete-by-reduction) gives others, one reduction at a time. Arora and Barak ([2009, sec. 2.4](#ref-arora2009computational)) prove [Theorem 6](#thm-indset-np-complete) this way, reducing a version of SAT to INDSET.

## 3 Worked reductions

> **NOTE:**
>
> **Definition 4 (The clique problem CLIQUE)** A *clique* in a graph is a set of vertices every two of which are joined by an edge. The decision problem **CLIQUE** asks: given a graph $G$ and an integer $k \ge 0$, does $G$ have a clique of $k$ vertices?

> **NOTE:**
>
> **Definition 5 (Complement of a graph)** The **complement** $\bar G$ of a graph $G$ has the same vertices as $G$, and joins two distinct vertices exactly when $G$ does not.

> **NOTE:**
>
> **Example 1 (From independent sets to cliques)** A set $S$ of vertices is independent in $G$ exactly when no two vertices of $S$ are joined in $G$, which is exactly when every two vertices of $S$ are joined in the [complement](#def-complement-graph) $\bar G$, that is, when $S$ is a [clique](#def-clique) in $\bar G$. So the function $f(G, k) = (\bar G, k)$ satisfies
>
> $$(G, k) \in \text{INDSET}
> \quad \text{if and only if} \quad
> (\bar G, k) \in \text{CLIQUE}.$$
>
> With $m$ vertices, computing $\bar G$ flips the $m(m - 1)$ off-diagonal entries of the adjacency matrix, which takes $O(m^2)$ steps. So INDSET $\le_p$ CLIQUE ([Definition 1](#def-reduction)). CLIQUE is in NP, since a clique is its own certificate, as in [the INDSET example](p-and-np.llms.md#exm-indset-in-np). INDSET is NP-complete ([Theorem 6](#thm-indset-np-complete)), so CLIQUE is NP-complete too, by [Theorem 4](#thm-np-complete-by-reduction).

> **NOTE:**
>
> **Definition 6 (The vertex cover problem VERTEX-COVER)** A *vertex cover* of a graph is a set of vertices that contains at least one end of every edge. The decision problem **VERTEX-COVER** asks: given a graph $G$ with $m$ vertices and an integer $k \ge 0$, does $G$ have a vertex cover of at most $k$ vertices?

> **NOTE:**
>
> **Exercise 1 (From independent sets to vertex covers)**  
>
> 1.  Show that a set $S$ of vertices is independent if and only if the set $\bar S$ of the vertices not in $S$ is a [vertex cover](#def-vertex-cover).
>
> 2.  Use part 1 to give a polynomial-time reduction ([Definition 1](#def-reduction)) from INDSET to VERTEX-COVER.

> **NOTE:**
>
> *Solution 1*.
>
> 1.  $S$ fails to be independent exactly when some edge has both its ends in $S$, which is exactly when some edge has no end in $\bar S$, that is, when $\bar S$ fails to be a vertex cover.
>
> 2.  Map $(G, k)$ to $(G, m - k)$ when $k \le m$, and to a fixed no-instance, such as a single edge with $k = 0$, when $k > m$. If $G$ has an independent set $S$ of $k$ vertices, then by part 1, $\bar S$ is a vertex cover of $m - k$ vertices. If $G$ has a vertex cover $C$ of at most $m - k$ vertices, then by part 1, $\bar C$ is independent with at least $k$ vertices, and any $k$ of them still form an independent set. So $(G, k) \in \text{INDSET}$ if and only if $(G, m - k) \in \text{VERTEX-COVER}$. The map copies $G$ and computes $m - k$, which takes polynomial time.

## 4 Hardness in data science

> **NOTE:**
>
> *Remark 2* (Sparse regression is NP-hard). Best-subset selection looks for the regression with the fewest nonzero coefficients that fits the data to a given tolerance. Its decision version asks: given a matrix $\mathbf{X}$, a vector $\mathord{\smash[b]{\underset{\raise.3ex\hbox{$\scriptstyle\sim$}}{y}}\vphantom{y}}$, a tolerance $\epsilon$ and an integer $k$, is there a vector $\mathord{\smash[b]{\underset{\raise.3ex\hbox{$\scriptstyle\sim$}}{\beta}}\vphantom{\beta}}$ with at most $k$ nonzero entries and $\|\mathbf{X}\mathord{\smash[b]{\underset{\raise.3ex\hbox{$\scriptstyle\sim$}}{\beta}}\vphantom{\beta}}- \mathord{\smash[b]{\underset{\raise.3ex\hbox{$\scriptstyle\sim$}}{y}}\vphantom{y}}\|_2 \le \epsilon$? Natarajan ([1995](#ref-natarajan1995sparse)) showed, by a reduction from an NP-complete problem, that this decision problem is NP-hard ([Definition 2](#def-np-complete)). An algorithm that finds a sparsest $\mathord{\smash[b]{\underset{\raise.3ex\hbox{$\scriptstyle\sim$}}{\beta}}\vphantom{\beta}}$ would decide it, by checking whether that $\mathord{\smash[b]{\underset{\raise.3ex\hbox{$\scriptstyle\sim$}}{\beta}}\vphantom{\beta}}$ has at most $k$ nonzero entries. If such an algorithm ran in polynomial time, [Theorem 3](#thm-np-complete-in-p) would put every problem in NP into P, and since [P is contained in NP](p-and-np.llms.md#thm-p-subset-np), P would equal NP. That is one reason practical methods use greedy forward selection or replace the count of nonzero coefficients with a convex penalty, as the lasso does.

## References

Arora, Sanjeev, and Boaz Barak. 2009. *Computational Complexity: A Modern Approach*. Cambridge University Press. <https://doi.org/10.1017/CBO9780511804090>.

Natarajan, B. K. 1995. “Sparse Approximate Solutions to Linear Systems.” *SIAM Journal on Computing* 24 (2): 227–34. <https://doi.org/10.1137/S0097539792240406>.

Back to top
