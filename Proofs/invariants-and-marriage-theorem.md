Your question is about the **proof methodology**, not specifically Hall's theorem or the Stable Marriage theorem. The underlying proof pattern is a general one:

> **Maintain an invariant under a transition system until termination. If a desired property were violated at termination, the invariant implies an impossible state.**

Let's formalize this from several viewpoints.

---

# 1. General Transition System

Let

* (S) be the state space.
* (T \subseteq S \times S) be the transition relation.
* (I : S \to \mathbf{Prop}) be an invariant.
* (F : S \to \mathbf{Prop}) denote terminal states.
* (G : S \to \mathbf{Prop}) denote the desired goal property.

The algorithm is

$$
s_0
\to
s_1
\to
\cdots
\to
s_n.
$$

---

## Invariant

An invariant satisfies

Initialization

$$
I(s_0)
$$

Preservation

$$
I(s)
\land
(s\to s')
\Longrightarrow
I(s').
$$

Hence

$$
I(s_i)
\quad
\forall i.
$$

---

## Termination

Suppose

$$
F(s_n).
$$

We wish to prove

$$
G(s_n).
$$

---

# 2. Contradiction Pattern

Instead of proving (G) directly,

assume

$$
\neg G(s_n).
$$

Then prove

$$
I(s_n)
\land
F(s_n)
\land
\neg G(s_n)
\Longrightarrow
\bot.
$$

Since

$$
I(s_n)
$$

already follows from induction,

the contradiction is forced.

Thus

$$
F(s_n)
\Longrightarrow
G(s_n).
$$

---

Notice the invariant is **not** itself the goal.

It is a stronger structural statement that survives every step.

---

# 3. Logical Skeleton

The proof has exactly this shape:

$$
\begin{aligned}
&I(s_0)\\
&I(s)\Rightarrow I(s')\\
&\therefore I(s_n)\\[4pt]
&F(s_n)\\
&\neg G(s_n)\\
&I(s_n)\land F(s_n)\land\neg G(s_n)\Rightarrow\bot\\
&\therefore G(s_n).
\end{aligned}
$$

---

# 4. Marriage Algorithm Example

Now instantiate.

State

$$
s=(M,\text{free men},\text{proposal history})
$$

where

* current matching
* rejected proposals
* proposal pointers

---

Invariant

One classical invariant is

> Every woman is matched to the best proposer she has seen so far.

Formally

$$
I(s)
====

\forall w,;
M(w)
====

\arg\max
{\text{proposers seen by }w}.
$$

This remains true after every proposal.

---

Terminal state

$$
F(s)
====

\text{no free man has an unproposed woman}.
$$

---

Goal

$$
G(s)
====

\text{matching is stable}.
$$

---

Contradiction

Assume

$$
\exists(m,w)
$$

forms a blocking pair.

Then

* (m) prefers (w),
* (w) prefers (m).

Because (m) prefers (w),

he must have proposed to her before proposing to his final partner.

Invariant:

When (w) rejected him,

she already held someone she preferred more.

Since she only improves,

her final partner is at least as good.

Hence

$$
w
\not>
M(w),
$$

contradiction.

Notice:

The contradiction uses the invariant—not merely the algorithm.

---

# 5. Category-Theoretic View

Treat the algorithm as a path in a category.

Objects:

states.

Morphisms:

algorithm steps.

```mermaid
flowchart LR

S0 --> S1
S1 --> S2
S2 --> S3
S3 --> Sn
```

Invariant

is a functor

$$
I:S\to\mathbf{Prop}
$$

such that every transition preserves truth.

Equivalently,

the following commutes:

```mermaid
flowchart LR

A[State s]
B[State s']

C[I(s)]
D[I(s')]

A --> B

A --> C
B --> D

C --> D
```

Meaning

$$
I(s)
\Longrightarrow
I(s').
$$

The invariant is transported along every morphism.

---

# 6. Hoare Logic View

This is exactly

$$
{I}
;
\texttt{step}
;
{I}
$$

meaning every iteration preserves (I).

Loop

$$
\texttt{while }B
{
\texttt{step}
}
$$

yields

$$
{I}
;
\texttt{loop}
;
{I\land\neg B}.
$$

Then show

$$
I\land\neg B
\Rightarrow
G.
$$

If easier,

prove

$$
I\land\neg B\land\neg G
\Rightarrow
\bot.
$$

---

# 7. Abstract Proof Schema

The proof technique can be summarized as a pipeline:

```mermaid
flowchart LR

A[Initial state]
-->B[Establish invariant]

B
-->C[Induction over every transition]

C
-->D[Invariant holds at termination]

D
-->E[Assume goal is false]

E
-->F[Invariant + terminal state imply contradiction]

F
-->G[Goal proven]
```

Formally, the proof is:

$$
\boxed{
\begin{aligned}
&\text{(Initialization)} && I(s_0),\
&\text{(Preservation)} && \forall s,s'.; I(s)\land T(s,s')\Rightarrow I(s'),\
&\text{(Termination)} && F(s_n),\
&\text{(Contradiction)} && I(s_n)\land F(s_n)\land\neg G(s_n)\Rightarrow\bot,\
&\therefore && F(s_n)\Rightarrow G(s_n).
\end{aligned}
}
$$

This is a ubiquitous proof pattern in algorithms: you establish an inductive invariant over a transition system, carry it to the terminal state by induction, and then use that invariant together with the terminal condition to rule out any counterexample to the desired property. The Gale–Shapley stable marriage algorithm, many graph algorithms, union–find correctness proofs, shortest-path algorithms, and numerous concurrent protocols all fit this schema.
