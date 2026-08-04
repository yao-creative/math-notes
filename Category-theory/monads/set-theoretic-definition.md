If you want to **quantify every component** of the monad triple in first-order/set-theoretic notation, you can write it as follows.

Let

[
\mathbf{Set}
]

denote the class of all sets.

A monad is a triple

[
M=(T,\eta,\mu)
]

where:

---

## 1. Functor

[
T=(T_0,T_1)
]

consists of two functions.

### Object map

[
T_0:\mathbf{Set}\rightarrow\mathbf{Set}
]

such that

[
\forall X\in\mathbf{Set},
\qquad
T_0(X)\in\mathbf{Set}.
]

---

### Morphism map

Let

[
\mathrm{Hom}(X,Y)
=================

{f\mid f:X\rightarrow Y}.
]

Then

[
T_1:
\bigcup_{X,Y}
\mathrm{Hom}(X,Y)
\rightarrow
\bigcup_{X,Y}
\mathrm{Hom}(T(X),T(Y))
]

with

[
\forall X,Y,
\forall f\in\mathrm{Hom}(X,Y),
]

[
T_1(f)
\in
\mathrm{Hom}(T(X),T(Y)).
]

subject to

[
T(\operatorname{id}_X)
======================

\operatorname{id}_{T(X)}
]

and

[
T(g\circ f)
===========

T(g)\circ T(f).
]

---

# 2. Unit

Rather than a single function,

[
\eta
====

{\eta_X}_{X\in\mathbf{Set}}
]

is an indexed family satisfying

[
\forall X\in\mathbf{Set},
]

there exists exactly one

[
\eta_X
\in
\mathrm{Hom}(X,T(X)).
]

Equivalently,

[
\eta
\in
\prod_{X\in\mathbf{Set}}
\mathrm{Hom}(X,T(X)).
]

The product means

> choose one function for every set.

---

# 3. Multiplication

Similarly,

[
\mu
===

{\mu_X}_{X\in\mathbf{Set}}
]

with

[
\forall X,
]

[
\mu_X
\in
\mathrm{Hom}(T(T(X)),T(X)).
]

Equivalently,

[
\mu
\in
\prod_{X\in\mathbf{Set}}
\mathrm{Hom}(T(T(X)),T(X)).
]

---

# 4. Entire quantified specification

Putting everything together,

[
\boxed{
\begin{aligned}
M=
(
&
T_0,
T_1,
\
&
\eta\in
\prod_{X}
\mathrm{Hom}(X,T(X)),
\
&
\mu\in
\prod_{X}
\mathrm{Hom}(T(T(X)),T(X))
)
\end{aligned}
}
]

subject to

[
\forall X,
]

[
\mu_X\circ\eta_{T(X)}
=====================

\operatorname{id}_{T(X)},
]

[
\mu_X\circ T(\eta_X)
====================

\operatorname{id}_{T(X)},
]

[
\mu_X\circ T(\mu_X)
===================

\mu_X\circ\mu_{T(X)}.
]

---

## Viewed purely as sets

You can think of each component as living in a different "type":

| Component | Set-theoretic type                                                                 |
| --------- | ---------------------------------------------------------------------------------- |
| (T_0)     | (\mathbf{Set}\to\mathbf{Set})                                                      |
| (T_1)     | (\displaystyle\prod_{X,Y}\left(\mathrm{Hom}(X,Y)\to\mathrm{Hom}(T(X),T(Y))\right)) |
| (\eta)    | (\displaystyle\prod_X\mathrm{Hom}(X,T(X)))                                         |
| (\mu)     | (\displaystyle\prod_X\mathrm{Hom}(T(T(X)),T(X)))                                   |

Notice that (\eta) and (\mu) are **not single functions on one set**; they are **indexed families of functions** (or equivalently, dependent products). Category theory packages these families as **natural transformations**, but from a set-theoretic perspective they are simply coherent choices of one function for every set (X).
