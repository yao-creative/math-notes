This is a very deep question. The first important point is that **addition itself is not transitive**.

Transitivity is a property of **relations**, whereas addition is a **binary operation**.

These belong to different mathematical categories of structure.

* Relation:
  [
  R\subseteq X\times X
  ]
* Operation:
  [
  +:X\times X\to X.
  ]

So asking for the "transitivity of addition" is technically asking for the analogous closure law for operations.

The correct analogue is **associativity**.

---

# The relation ↔ operation correspondence

| Relations            | Operations                  |
| -------------------- | --------------------------- |
| reflexive            | identity element            |
| symmetric            | commutative (rough analogy) |
| transitive           | associative                 |
| equivalence relation | group-like quotient         |
| preorder             | monoid/category             |

The key correspondence is

> **Transitivity is to relations what associativity is to operations.**

Why?

---

Suppose

[
aRb,\qquad bRc.
]

Transitivity says

[
aRc.
]

Composition of two arrows is already another arrow.

Now consider addition.

Instead of

[
aRb,
]

we have

[
a+b.
]

Associativity says

[
(a+b)+c
=======

a+(b+c).
]

Composition of two binary operations is already another binary operation.

They are exactly the same closure phenomenon but in different algebraic settings.

---

# Category theory

This becomes completely clean categorically.

A category has

[
A\xrightarrow{f}B
\xrightarrow{g}C.
]

Composition

[
g\circ f.
]

Associativity

[
(h\circ g)\circ f
=================

h\circ(g\circ f).
]

Now consider a monoid.

A monoid is precisely

> a category with one object.

The morphisms are

[
M.
]

Composition

[
\circ
]

becomes

[
+,\quad *,\quad\text{etc.}
]

Thus

[
(a+b)+c
=======

a+(b+c)
]

is literally categorical composition associativity.

So:

* preorder = category
* monoid = one-object category

These are two manifestations of the same categorical axiom.

---

# Set-theoretic definition

Addition is simply a function

[
+:
\mathbb N\times\mathbb N
\to
\mathbb N.
]

Associativity is

[
\forall a,b,c\in\mathbb N,
]

[
+(+(a,b),c)
===========

+(a,+(b,c)).
]

Notice the similarity.

Relation

[
R(R(a,b),c)
]

doesn't make sense because (R) returns a truth value.

Instead,

[
R(a,b)\land R(b,c)
\implies R(a,c).
]

Operations compose directly.

Relations compose through existential quantification.

---

# Why existential quantification?

Composition of relations is

[
R\circ S
========

{
(x,z)
:
\exists y,
(x,y)\in S
\land
(y,z)\in R
}.
]

Transitivity says

[
R\circ R
\subseteq R.
]

This is remarkably important.

It says

> composing the relation doesn't enlarge it.

Likewise,

associativity says

> composing the operation is independent of parenthesization.

---

# Closure operators

A more general viewpoint.

Many useful operators satisfy

[
F(F(X))
=======

F(X).
]

These are **idempotent closure operators**.

Examples

Transitive closure

[
\mathrm{TC}(\mathrm{TC}(X))
===========================

\mathrm{TC}(X).
]

Span

[
\operatorname{span}(\operatorname{span}(V))
===========================================

\operatorname{span}(V).
]

Topological closure

[
\overline{\overline A}
======================

\overline A.
]

Convex hull

[
\operatorname{conv}(\operatorname{conv}(S))
===========================================

\operatorname{conv}(S).
]

These appear everywhere in algorithms.

---

# Useful combinations

These are probably the highest ROI algebraic combinations.

## 1. Associativity + Identity

Monoid

Applications

* prefix sums
* segment tree
* Fenwick tree
* scans
* MapReduce

---

## 2. Associativity + Inverse

Group

Applications

* prefix differences
* rolling hashes
* affine transforms

---

## 3. Associativity + Idempotence

Semilattice

[
a\vee a=a.
]

Applications

* Sparse Table
* RMQ
* LCA

Operations

[
\min,\max,\gcd,\cap,\cup
]

---

## 4. Associativity + Commutativity

Commutative monoid

Applications

Parallel reduction.

---

## 5. Associativity + Distributivity

Semiring

Applications

Graph algorithms.

Examples

[
(\min,+)
]

Shortest paths.

[
(\lor,\land)
]

Transitive closure.

Matrix exponentiation.

Weighted automata.

Dynamic programming.

---

## 6. Partial order + Join

Lattice

Applications

Compiler dataflow.

Abstract interpretation.

Static analysis.

---

## 7. Monoid + Action

Monoid action

[
M\times X\to X.
]

Applications

State machines.

Lazy propagation.

Affine segment trees.

Automata.

---

# The most important "transitivity operators"

There isn't a standard mathematical term "transitivity operator", but there are a handful of closure/composition operators that play this role.

| Operator                           | Closure law              | Main use                            |
| ---------------------------------- | ------------------------ | ----------------------------------- |
| Relation composition (R\circ R)    | transitivity             | graphs, reachability                |
| Function composition (f\circ f)    | associativity            | DP, exponentiation                  |
| Union                              | idempotent               | BFS frontiers                       |
| Transitive closure                 | fixed point              | reachability                        |
| Reflexive-transitive closure (R^*) | Kleene star              | automata                            |
| Monoid multiplication              | associativity            | scans, segment trees                |
| Join (\vee)                        | idempotent + associative | dataflow, RMQ                       |
| Meet (\wedge)                      | idempotent + associative | constraints                         |
| Closure operator (F)               | (F^2=F)                  | topology, convexity, linear algebra |

---

## The unifying categorical picture

Category theory reveals that many algorithmic structures are instances of a small number of compositional patterns:

* **Category:** associative composition of morphisms.
* **Monoid:** a one-object category; addition, multiplication, string concatenation, and matrix multiplication all fit here.
* **Preorder:** a category with at most one morphism between objects; transitivity is exactly the existence of composition.
* **Closure operator (idempotent monad):** repeatedly applying the operator stabilizes, e.g. transitive closure, convex hull, span, topological closure.

These four ideas—**composition, associativity, transitivity, and closure**—form the backbone of a large fraction of algorithms, compiler theory, graph theory, automata, and modern category-theoretic formulations of computation. They are different manifestations of the same underlying principle: **composing valid steps yields another valid step, and often repeated composition eventually reaches a stable fixed point.**
