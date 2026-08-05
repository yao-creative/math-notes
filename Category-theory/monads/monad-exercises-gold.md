Since you're interested in understanding monads from **set theory → universal algebra → category theory**, here are exercises ordered by dependency. Each one requires constructing the object rather than recalling definitions.

---

# Level 1 — Set-Theoretic Foundations

### Exercise 1 — Hom Sets

Given

[
X={1,2},\qquad
Y={a,b,c},
]

1. Describe (\mathrm{Hom}(X,Y)).
2. How many elements does it contain?
3. Construct three distinct members.

---

### Exercise 2 — Set Transformer

Define

[
T(X)=X\cup{\bot}.
]

For

[
X={1,2},
]

compute

[
T(X).
]

Then for

[
f:X\rightarrow Y
]

where

[
f(1)=a,\qquad
f(2)=b,
]

construct

[
T(f).
]

---

### Exercise 3 — List Functor

Let

[
X={a,b}.
]

Compute

[
T(X)
]

where

[
T(X)=\text{finite lists over }X.
]

Write the image of

[
[a,b,a]
]

under

[
T(f)
]

for

[
f(a)=1,\qquad
f(b)=2.
]

---

# Level 2 — Functor Laws

Prove directly that

[
T(\operatorname{id}_X)
======================

\operatorname{id}_{T(X)}
]

for

* Maybe
* List
* Powerset

without mentioning category theory.

---

Prove

[
T(g\circ f)
===========

T(g)\circ T(f)
]

for the List functor.

---

# Level 3 — Construct Natural Transformations

For the Maybe monad

construct

[
\eta_X
:
X\rightarrow X\cup{\bot}.
]

---

Construct

[
\mu_X
:
(X\cup{\bot})\cup{\bot}
\rightarrow
X\cup{\bot}.
]

Give every possible case.

---

Repeat both for

[
T(X)=\mathcal P(X).
]

---

# Level 4 — Monad Laws

Without using Rust or Haskell.

Prove

[
\mu_X\circ\eta_{T(X)}
=====================

\operatorname{id}_{T(X)}.
]

---

Prove

[
\mu_X\circ T(\eta_X)
====================

\operatorname{id}_{T(X)}.
]

---

Prove associativity

[
\mu_X\circ T(\mu_X)
===================

\mu_X\circ\mu_{T(X)}
]

for the List monad.

---

# Level 5 — Invent Your Own Monad

Suppose

[
T(X)=X\times\mathbb N.
]

Questions:

1. Can you define

[
\eta_X?
]

2. Can you define

[
\mu_X?
]

3. Does associativity hold?

---

Repeat for

[
T(X)=X\times M
]

where

[
(M,\cdot,e)
]

is a monoid.

Show exactly where the monoid laws appear.

---

# Level 6 — Kleisli Category

Suppose

[
f:A\rightarrow T(B)
]

and

[
g:B\rightarrow T(C).
]

Construct

[
g\star f.
]

Show why ordinary composition

[
g\circ f
]

is not well typed.

---

Repeat for

* Maybe
* List
* Powerset

---

# Level 7 — Universal Algebra

A group is

[
(G,\cdot,e,^{-1}).
]

A monad is

[
(T,\eta,\mu).
]

Answer:

1. Which symbols are operations?
2. What are the carrier sets?
3. What are the equations?
4. Why is a monad **not** an algebra on one set?

---

# Level 8 — Category Theory

Given

[
\eta:\mathrm{Id}\Rightarrow T,
]

write the naturality square explicitly:

[
\begin{array}{ccc}
X & \xrightarrow{f} & Y\
\downarrow{\eta_X} && \downarrow{\eta_Y}\
T(X) & \xrightarrow{T(f)} & T(Y)
\end{array}
]

and prove it commutes.

---

Do the same for

[
\mu:T^2\Rightarrow T.
]

---

# Level 9 — Rust Translation

Translate each of the following Rust APIs into categorical notation:

```rust
Option<T>
```

```rust
Result<T,E>
```

```rust
Vec<T>
```

```rust
Iterator<Item = T>
```

For each identify:

* the endofunctor (T),
* the unit (\eta),
* the multiplication (\mu) (or equivalently, `flatten`),
* the bind (`and_then`/`flat_map`) operation.

---

# Final Challenge (Research-Level)

Suppose someone hands you only the operation

[
\operatorname{bind}:
T(X)\times(X\to T(Y))
\to
T(Y).
]

Can you reconstruct:

1. the unit (\eta),
2. the multiplication (\mu),
3. the Kleisli category,
4. and prove the monad laws from the bind laws?

This is a deep exercise because it shows that the two common presentations of a monad—((T,\eta,\mu)) and ((T,\eta,\mathsf{bind}))—are mathematically equivalent. Solving it demonstrates a structural understanding of monads rather than familiarity with a particular programming interface.
