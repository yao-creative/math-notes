Yes. Many algorithmic "tricks" are really **transformations of logical statements or set operations**. Prefix sums are one example (changing representation). Other families come from manipulating:

$$
\text{predicates}
\leftrightarrow
\text{sets}
\leftrightarrow
\text{relations}
\leftrightarrow
\text{functions}
$$

Below are the major ones.

---

# 1. Complement transformation (solve the opposite)

Set identity:

$$
A=B^c{}^c
$$

Logic:

$$
P\equiv\neg\neg P
$$

Sometimes the complement has simpler structure.

---

## Example: Sliding window

Problem:

Find the longest substring satisfying:

$$
\forall i,; condition(i)
$$

Instead track failure:

$$
\exists i,;\neg condition(i)
$$

The window expands until:

$$
\text{bad elements}=0
$$

You are maintaining:

$$
W\cap B=\varnothing
$$

where:

* (W) = current window
* (B) = bad elements

---

# 2. De Morgan's laws (turn AND into OR)

Set form:

$$
(A\cap B)^c=A^c\cup B^c
$$

Logic:

$$
\neg(P\land Q)=\neg P\lor\neg Q
$$

---

## Example: Validation

Instead of:

$$
valid=
A\land B\land C\land D
$$

check:

$$
invalid=
A^c\lor B^c\lor C^c\lor D^c
$$

This gives early termination.

Used in:

* parsers
* validators
* constraint solving

---

# 3. Existential ↔ universal duality

Logic:

$$
\neg\exists x:P(x)
\iff
\forall x:\neg P(x)
$$

---

## Example: Binary search

Finding:

$$
\exists x:f(x)\geq k
$$

becomes:

Find first position where:

$$
\forall i<x:f(i)<k
$$

This creates monotonic predicates.

Binary search works because:

$$
false,false,false,true,true,true
$$

is an ordered partition of the set.

---

# 4. Equivalence classes (quotienting)

Define relation:

$$
x\sim y
\iff
f(x)=f(y)
$$

Then:

$$
X/\sim
$$

groups objects with the same behavior.

---

## Example: Hash maps

Instead of storing:

$$
X
$$

store:

$$
X/\sim
$$

where:

$$
x\sim y
\iff
hash(x)=hash(y)
$$

The hash function is:

$$
h:X\rightarrow H
$$

and buckets are fibers:

$$
h^{-1}(k)
=========

{x:h(x)=k}
$$

---

# 5. Invariants (subsets closed under operations)

An invariant is a subset:

$$
I\subseteq S
$$

such that:

$$
f(I)\subseteq I
$$

Meaning:

once inside (I), transitions cannot escape.

---

## Example: Loop invariant

Loop state:

$$
s_i
$$

Transition:

$$
f:S\rightarrow S
$$

Need:

$$
s_0\in I
$$

and:

$$
\forall s\in I:f(s)\in I
$$

Therefore:

$$
\forall i:s_i\in I
$$

This is the mathematical form of correctness proofs.

---

# 6. Pigeonhole principle

Set cardinality:

If:

$$
|A|>|B|
$$

then no injective function exists:

$$
f:A\rightarrow B
$$

---

## Example: Duplicate detection

If:

$$
n+1
$$

objects placed into:

$$
n
$$

buckets:

$$
\exists x\neq y:f(x)=f(y)
$$

Used in:

* cycle detection
* hashing
* birthday paradox problems

---

# 7. Relations as graphs

A relation:

$$
R\subseteq A\times B
$$

is a set of pairs.

Algorithms often manipulate relations.

---

## Example: BFS

Graph:

$$
E\subseteq V\times V
$$

Reachability is:

$$
R^*
$$

the transitive closure:

$$
R\cup R^2\cup R^3...
$$

BFS computes this closure efficiently.

---

# 8. Partial orders and minimal elements

A partial order:

$$
\leq\subseteq X\times X
$$

with:

* reflexive
* antisymmetric
* transitive

Many greedy algorithms find minimal/maximal elements.

---

## Example: Topological sort

A DAG defines:

$$
u\leq v
$$

if:

$$
u\leadsto v
$$

The algorithm repeatedly removes minimal elements:

$$
{x:\nexists y(y<x)}
$$

---

# 9. Cardinality arguments

Instead of constructing an object, prove it exists.

Example:

Need:

$$
\exists x\in X:P(x)
$$

Show:

$$
|X|>|X\setminus P|
$$

Therefore:

$$
P\neq\varnothing
$$

Used in:

* randomized algorithms
* lower bounds
* combinatorics

---

# 10. Function composition / factorization

Instead of computing:

$$
f:X\rightarrow Z
$$

directly:

factor:

$$
X\xrightarrow{g}Y\xrightarrow{h}Z
$$

where:

$$
f=h\circ g
$$

Examples:

* compiler pipelines
* ML pipelines
* FFT
* database query optimization

---

# Summary table

| Logic/set trick        | Algorithm pattern                   |
| ---------------------- | ----------------------------------- |
| Complement             | sliding window, two pointers        |
| De Morgan              | validation, pruning                 |
| Quantifier negation    | binary search, impossibility proofs |
| Equivalence classes    | hashing, union-find                 |
| Invariant subsets      | loop correctness                    |
| Pigeonhole             | duplicate/cycle detection           |
| Relations              | graphs, BFS/DFS                     |
| Partial orders         | greedy, scheduling                  |
| Cardinality            | existence proofs                    |
| Function factorization | pipelines, transformations          |

The deeper pattern is:

$$
\boxed{
\text{Algorithm design}
=======================

\text{Find an equivalent mathematical object where the structure is easier}
}
$$

The "trick" is rarely the code itself; it is usually discovering the right **set-theoretic representation** of the problem.
