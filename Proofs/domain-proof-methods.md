You are asking for a **formal taxonomy of proof schemas**: not the examples, but the abstract mathematical form each pattern instantiates.

A useful abstraction is:

* A problem domain has a state/object space (X).
* A proof pattern is an operator that transforms assumptions into conclusions.
* Most patterns are compositions of:

  * implication
  * induction
  * optimization
  * order theory
  * algebraic structure
  * fixed points

I will write each as:

$$
\textbf{Name}
$$

$$
\text{Structure}
$$

$$
\text{Proof schema}
$$

---

# 1. Greedy Algorithms

## Exchange Argument

### Structure

Solution space:

$$
\mathcal S
$$

Objective:

$$
f:\mathcal S\rightarrow \mathbb R
$$

Optimal solutions:

$$
\mathrm{OPT}
=

{s:f(s)=\max f}.
$$

### Schema

Show:

$$
\forall o\in\mathrm{OPT},
\exists o'
$$

such that

$$
f(o')=f(o)
$$

and

$$
\mathrm{distance}(o',G(o))
<
\mathrm{distance}(o,G(o)).
$$

Repeated exchange:

$$
o\rightarrow o_1\rightarrow...\rightarrow G.
$$

Therefore:

$$
G\in\mathrm{OPT}.
$$

---

# 2. Greedy Stays Ahead

Define progress measure:

$$
A_i,O_i
$$

where:

* (A_i) = greedy prefix
* (O_i) = optimal prefix

Show invariant:

$$
\forall i:
A_i\preceq O_i
$$

where (\preceq) means "at least as good".

Then:

$$
A_n\preceq O_n
$$

so greedy is optimal.

---

# 3. Cut Property (Graphs)

Graph:

$$
G=(V,E)
$$

Cut:

$$
(S,V-S)
$$

Weight:

$$
w:E\rightarrow\mathbb R
$$

Schema:

For minimum crossing edge:

$$
e=\arg\min_{u\in S,v\notin S}w(u,v)
$$

prove:

$$
\exists MST\ M:
e\in M.
$$

Proof uses exchange:

$$
M-e+e'
$$

preserves spanning tree and improves weight.

---

# 4. Dynamic Programming

## Bellman Principle

State space:

$$
X
$$

Value function:

$$
V:X\rightarrow\mathbb R
$$

Optimality equation:

$$
V(x)
====

\min_a
{
c(x,a)+V(T(x,a))
}.
$$

Proof:

Show recurrence satisfies:

$$
V(x)\leq C(\pi)
$$

for every policy (\pi).

Then construct policy attaining equality.

---

# 5. Graph Reachability

Graph:

$$
G=(V,E)
$$

Reachability:

$$
R(v)
$$

Base:

$$
R(s)
$$

Closure:

$$
R(u)\land(u,v)\in E
\Rightarrow
R(v)
$$

Therefore:

$$
R=\mathrm{TC}(E)
$$

(transitive closure).

---

# 6. Shortest Path

Distance:

$$
d(v)
$$

Relaxation:

$$
d(v)
\leq
d(u)+w(u,v)
$$

Invariant:

$$
d(v)\leq d^*(v)
$$

and eventually:

$$
d(v)=d^*(v).
$$

---

# 7. Matching

## Augmenting Path

Matching:

$$
M\subseteq E
$$

Alternating path:

$$
P=e_1,e_2,...,e_k
$$

with:

$$
e_i\in M
\iff
i\text{ even}.
$$

Augment:

$$
M'=M\triangle P
$$

Then:

$$
|M'|=|M|+1.
$$

Termination:

$$
\nexists P
\iff
M\text{ maximum}.
$$

---

# 8. Hall Deficiency

Bipartite graph:

$$
G=(L,R,E)
$$

Define:

$$
\delta(S)=|N(S)|-|S|.
$$

If:

$$
\exists S:
\delta(S)<0
$$

then:

$$
\text{no perfect matching}.
$$

Conversely:

$$
\forall S:
\delta(S)\geq0
$$

implies matching exists.

---

# 9. Network Flow

Flow:

$$
f:E\rightarrow\mathbb R
$$

Constraints:

Capacity:

$$
0\leq f(e)\leq c(e)
$$

Conservation:

$$
\sum f_{in}
=

\sum f_{out}.
$$

Residual graph:

$$
G_f.
$$

Invariant:

$$
\text{augmenting path exists}
\Rightarrow
\text{flow not maximal}.
$$

---

# 10. Matroid Basis Exchange

Matroid:

$$
M=(E,\mathcal I)
$$

Independent sets:

$$
A,B\in\mathcal I.
$$

Exchange axiom:

$$
|A|<|B|
\Rightarrow
\exists x\in B-A:
A+x\in\mathcal I.
$$

This gives greedy correctness.

---

# 11. Approximation Algorithms

## Charging

Algorithm cost:

$$
C_A
$$

Optimal:

$$
C^*
$$

Define:

$$
\phi:A\rightarrow O
$$

where (O) is optimal elements.

Bound:

$$
\sum_{a\in A}\phi(a)
\leq
\alpha C^*
$$

therefore:

$$
C_A\leq\alpha C^*.
$$

---

# 12. Online Algorithms

Competitive ratio:

$$
\frac{C_A(\sigma)}
{C_{OPT}(\sigma)}
\leq
\alpha
$$

for all inputs:

$$
\sigma.
$$

---

# 13. Amortized Analysis

Potential:

$$
\Phi:S\rightarrow\mathbb R_{\ge0}
$$

Actual cost:

$$
c_i
$$

Amortized:

$$
\hat c_i
=

c_i+\Phi_i-\Phi_{i-1}
$$

Summation:

$$
\sum\hat c_i
=

\sum c_i+\Phi_n-\Phi_0.
$$

---

# 14. Concurrent Algorithms

## Safety Invariant

State:

$$
s\in S
$$

Bad states:

$$
B\subseteq S
$$

Prove:

$$
I(s_0)
$$

and

$$
I(s)\land T(s,s')
\Rightarrow I(s')
$$

and:

$$
I\cap B=\emptyset.
$$

---

# 15. Linearizability

Concurrent history:

$$
H
$$

Sequential history:

$$
S
$$

Need:

$$
H\sim S
$$

where equivalence preserves observable behavior.

Find linearization point:

$$
t_{lin}.
$$

---

# 16. Cryptographic Hybrid Argument

Games:

$$
G_0,G_1,...,G_n
$$

Show:

$$
|\Pr[G_i]-\Pr[G_{i+1}]|
\leq\epsilon_i
$$

Then:

$$
|\Pr[G_0]-\Pr[G_n]|
\leq
\sum_i\epsilon_i.
$$

---

# 17. Probabilistic Method

Random variable:

$$
X
$$

Show:

$$
E[X]>0.
$$

Therefore:

$$
\exists x:X(x)>0.
$$

Existence follows from expectation.

---

# 18. Double Counting

Set:

$$
S
$$

Count function:

$$
|S|
$$

two ways:

$$
|S|
=
\sum_a n_a
=
\sum_b m_b.
$$

Hence:

$$
\sum_a n_a=\sum_bm_b.
$$

---

# 19. Algebraic Proof (Universal Property)

Object:

$$
X
$$

Need show unique morphism:

$$
f:X\rightarrow Y.
$$

Existence:

$$
\exists f.
$$

Uniqueness:

$$
f=g.
$$

Therefore:

$$
\exists !f.
$$

---

# 20. Category-Theoretic Diagram Chase

Given commutative diagram:

$$
g\circ f=h\circ k
$$

derive unknown morphism:

$$
x
$$

using:

* composition
* cancellation
* universal properties.

---

# 21. Topological Invariance

Invariant:

$$
I(X)
$$

under transformation:

$$
f:X\rightarrow Y.
$$

Need:

$$
I(X)=I(Y)
$$

whenever:

$$
X\simeq Y.
$$

Examples:

$$
\pi_1(X)
$$

is homotopy invariant.

---

# 22. Optimization Duality

Primal:

$$
\max c^Tx
$$

Dual:

$$
\min b^Ty
$$

Weak duality:

$$
c^Tx\le b^Ty.
$$

Strong duality:

$$
\exists x^*,y^*
:
c^Tx^*=b^Ty^*.
$$

---

# The meta-pattern behind all of them

Almost every domain proof is one of these transformations:

$$
\boxed{
\text{Structure}
\rightarrow
\text{Invariant}
\rightarrow
\text{Monotone quantity}
\rightarrow
\text{Fixed point / contradiction}
\rightarrow
\text{Theorem}
}
$$

More abstractly:

$$
\text{Problem domain}
$$

gives you a space:

$$
X
$$

a relation/order:

$$
\leq
$$

and a preserved object:

$$
I:X\rightarrow\mathrm{Prop}.
$$

Then correctness is usually proving:

$$
\boxed{
\mathrm{Reach}(x_0)
\subseteq
I
}
$$

and

$$
\boxed{
I\cap\neg G=\emptyset.
}
$$

This is the common algebraic skeleton behind algorithms, verification, combinatorics, optimization, and many mathematical proofs.
