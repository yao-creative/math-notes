Excellent question. What you're asking is a **proof-theoretic dependency** question:

> **Why are we allowed to "bring in" the invariant at the end of the proof? Is it an extra axiom, or does it logically depend on the execution?**

The answer is: **an invariant is never an axiom.** It is a theorem derived by induction over the execution. By the time you reach the terminal state, you have already proved (I(s_n)).

---

## The dependency graph

The logical dependencies are

```mermaid
flowchart TD

A[Initial state s₀]
-->B[I(s₀)]

B
-->C[Induction]

D[Transition preserves I]
-->C

C
-->E[I(sₙ)]

F[Termination]
-->G[F(sₙ)]

E
-->H[Contradiction]

G
-->H

I["¬G(sₙ)"]
-->H

H
-->J["⊥"]

J
-->K["G(sₙ)"]
```

Notice

$$
I(s_n)
$$

is **not assumed**.

It has already been established before the contradiction begins.

---

# The invariant is an inductive theorem

Suppose

$$
s_0
\to
s_1
\to
\cdots
\to
s_n.
$$

You prove

Initialization

$$
I(s_0).
$$

Preservation

$$
I(s_i)
\Rightarrow
I(s_{i+1}).
$$

Then ordinary induction gives

$$
\forall i,;
I(s_i).
$$

This is now a theorem.

Since

$$
s_n
$$

is one of those states,

$$
I(s_n)
$$

is simply obtained by universal instantiation.

Nothing magical happened.

---

# There is no dependence between (I) and (F)

This is the subtle point.

Usually

$$
I
$$

and

$$
F
$$

are **independent predicates** on the state.

Think of

State

$$
S.
$$

Then

$$
I,F,G:S\to\mathbf{Prop}.
$$

Graphically,

$$
\begin{aligned}
S
&\xrightarrow{I}
\mathbf{Prop}\\
S
&\xrightarrow{F}
\mathbf{Prop}\\
S
&\xrightarrow{G}
\mathbf{Prop}.
\end{aligned}
$$

They simply observe different properties of the same state.

Neither implies the other in general.

---

# What actually depends on what?

The real dependency is

$$
\boxed{
I
+
F
\Longrightarrow
G.
}
$$

or, contrapositively,

$$
I
+
F
+
\neg G
\Longrightarrow
\bot.
$$

So

* the invariant gives historical information,
* the terminal condition gives local information,
* together they imply correctness.

Neither is sufficient alone.

---

# Gale–Shapley example

Invariant

> Every woman holds her favorite proposer so far.

Terminal

> No man has anyone left to propose to.

Neither implies stability individually.

Example:

Invariant without termination:

A blocking pair may still exist because proposals are incomplete.

Termination without invariant:

The algorithm could terminate in a completely unstable matching.

Only together

$$
I
\land
F
$$

force stability.

---

# Hoare Logic viewpoint

A loop proof is

$$
{I}
;
\texttt{while }B
{
C
}
;
{I\land\neg B}.
$$

Notice the postcondition is

$$
I\land\neg B.
$$

Not merely

$$
\neg B.
$$

The invariant survives the loop.

So when proving the final theorem,

you already possess

$$
I.
$$

---

# A categorical perspective

Think of the execution as a morphism

$$
e:s_0\to s_n.
$$

The invariant is not attached to the terminal state alone; it is **transported** along the execution. Formally, if we view predicates as a fibration over the category of states, the preservation condition says that (I) is stable under every transition. The execution induces

$$
I(s_0)
;\Longrightarrow;
I(s_n).
$$

So (I(s_n)) is the image of the initial proof under repeated transport along the execution.

---

# A useful design principle

When constructing correctness proofs, it's often helpful to distinguish three kinds of information:

* **Initial facts**: true only at the start (e.g., the input graph is bipartite).
* **Invariants**: facts preserved by every transition (e.g., every woman holds her best proposer so far).
* **Terminal conditions**: facts true only when the algorithm stops (e.g., no unmatched man has anyone left to propose to).

The proof of correctness then has the shape

$$
\text{Initial facts}
;\xRightarrow{\text{induction}};
I
\quad\text{and}\quad
F,
$$

followed by

$$
I \land F ;\Longrightarrow; G.
$$

The invariant is therefore **not an axiom** and generally **not dependent on termination**. It is an **inductively derived property of every reachable state**, and the terminal state is simply one reachable state where you combine that derived property with the termination condition to conclude the desired result.
