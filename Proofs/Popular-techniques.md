This is essentially asking for an **algebra of predicates**—common ways to compose predicates that correspond to recurring proof techniques. Many algorithmic, combinatorial, and verification proofs reduce to instantiating one of these schemas.

Let (P,Q,R,I,F,G : X \to \mathbf{Prop}) be predicates on some state space (X).

---

# 1. Induction (Invariant Propagation)

The fundamental schema:

$$
\boxed{
P(x_0)
\land
\forall x,x'.,
(P(x)\land T(x,x'))
\Rightarrow
P(x')
}
$$

yields

$$
\boxed{
\forall x\in\mathrm{Reach}(x_0),;
P(x).
}
$$

Interpretation:

* (P) is an inductive invariant.
* This is the basis of loop invariants, graph searches, distributed protocols, etc.

---

# 2. Invariant + Terminal ⇒ Goal

The standard correctness composition:

$$
\boxed{
I
\land
F
\Rightarrow
G.
}
$$

Equivalent contradiction form:

$$
\boxed{
I
\land
F
\land
\neg G
\Rightarrow
\bot.
}
$$

This is the Gale–Shapley pattern.

---

# 3. Strengthening

Instead of proving

$$
P\Rightarrow G,
$$

prove

$$
P\Rightarrow Q
$$

and

$$
Q\Rightarrow G.
$$

Composition:

$$
\boxed{
(P\Rightarrow Q)
\land
(Q\Rightarrow G)
\Rightarrow
(P\Rightarrow G).
}
$$

Very common in induction.

---

# 4. Weakening

Sometimes a stronger invariant is difficult.

Use

$$
P
\Rightarrow
Q.
$$

Then

$$
Q
$$

is sufficient.

---

# 5. Case Split

Partition the space.

$$
\boxed{
(P\land A\Rightarrow G)
\land
(P\land\neg A\Rightarrow G)
}
$$

implies

$$
P\Rightarrow G.
$$

---

# 6. Contrapositive

Replace

$$
P\Rightarrow Q
$$

by

$$
\boxed{
\neg Q
\Rightarrow
\neg P.
}
$$

Often much easier.

---

# 7. Contradiction

Assume

$$
P
\land
\neg Q.
$$

Show

$$
\bot.
$$

Equivalent to

$$
P\Rightarrow Q.
$$

---

# 8. Minimal Counterexample

Assume

$$
\exists x,;
\neg P(x).
$$

Choose

$$
x_0
=

\min
{
x:\neg P(x)
}.
$$

Then derive

$$
\exists y<x_0
$$

also violating (P).

Contradiction.

---

# 9. Monotonicity

Suppose

$$
P(x)
$$

is monotone.

Then

$$
x\le y
\Rightarrow
P(x)
\Rightarrow
P(y).
$$

Useful in lattice algorithms.

---

# 10. Closure

Show

$$
P
$$

is preserved by an operation

$$
f.
$$

$$
\boxed{
P(x)
\Rightarrow
P(f(x)).
}
$$

Repeated application gives

$$
P(f^n(x)).
$$

---

# 11. Ranking Function

Termination.

Find

$$
r:S\to\mathbb N
$$

such that

$$
r(s')<r(s)
$$

every step.

Then

$$
\boxed{
r
\text{ strictly decreases}
\Longrightarrow
\text{termination}.
}
$$

---

# 12. Progress + Safety

Concurrency.

Safety

$$
I.
$$

Progress

$$
\Diamond G.
$$

Correctness

$$
I
\land
\Diamond G.
$$

---

# 13. Mutual Exclusion

Two predicates

$$
P,Q.
$$

Need

$$
\boxed{
P
\land
Q
\Rightarrow
\bot.
}
$$

---

# 14. Uniqueness

Need

$$
\exists x,P(x)
$$

and

$$
P(x)
\land
P(y)
\Rightarrow
x=y.
$$

Then

$$
\exists!
x,P(x).
$$

---

# 15. Maximality

Suppose

$$
M(x).
$$

Need

$$
\boxed{
M(x)
\land
\text{can extend}
\Rightarrow
\bot.
}
$$

Hence

$$
M
$$

is maximal.

---

# 16. Exchange

Greedy algorithms.

Maintain

$$
P
$$

and prove

$$
P
\land
\text{non-optimal}
\Rightarrow
\exists
\text{exchange}
$$

that preserves

$$
P.
$$

Eventually

$$
P
\land
\text{optimal}.
$$

---

# 17. Local ⇒ Global

Very common.

$$
\boxed{
\forall x,;
L(x)
}
\Rightarrow
G.
$$

Examples

* convexity
* matroids
* stable marriage
* shortest paths

---

# 18. Preservation + Reflection

Category theory language.

A map

$$
f:X\to Y
$$

preserves

$$
P
$$

if

$$
P(x)
\Rightarrow
Q(f(x)).
$$

Reflects

$$
P
$$

if

$$
Q(f(x))
\Rightarrow
P(x).
$$

An isomorphism gives both.

---

# 19. Hoare Triple Composition

$$
{P}
C
{Q}
$$

and

$$
{Q}
D
{R}
$$

compose into

$$
\boxed{
{P}
C;D
{R}.
}
$$

---

# 20. Reachability Composition

Let

$$
R(x)
====

\text{reachable}(x).
$$

Then

$$
R
\land
I
\Rightarrow
\text{legal}.
$$

Almost every state-machine proof has this form.

---

# A Unifying Meta-Pattern

Many proof techniques can be organized into the following compositional pipeline:

$$
\boxed{
\underbrace{\text{Initial}}*{\text{axioms}}
;\xRightarrow{\text{induction}};
\underbrace{I}*{\text{persistent}}
;\land;
\underbrace{F}*{\text{terminal}}
;\xRightarrow{\text{deduction or contradiction}};
\underbrace{G}*{\text{goal}}.
}
$$

In category-theoretic terms, if predicates on a state space (S) form a preorder ((\mathrm{Pred}(S),\Rightarrow)), then implication is the order relation, conjunction is the meet ((\wedge)), and many proof techniques amount to constructing a path in this preorder:

$$
\text{Initial}
;\le;
I
;\wedge;
F
;\le;
G.
$$

Viewed this way, induction, contradiction, case analysis, strengthening, weakening, exchange arguments, and ranking functions are different methods for establishing the required morphisms (implications) in the predicate lattice. This perspective unifies a wide range of correctness proofs in algorithms, combinatorics, and program verification.
