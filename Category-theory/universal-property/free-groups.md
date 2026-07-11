The two concepts are connected:

* A **free group** answers: *"Given generators, what is the most general group containing them?"*
* An **extension** answers: *"Given two groups, how can one be glued on top of the other?"*

Together they explain much of how groups are **constructed**.

---

# 1. Free group

## Intuition

Suppose you have a set of symbols:

$$
X={a,b,c}
$$

You want a group containing these symbols, but you do not want to impose any extra relations.

The free group is:

$$
F(X)
$$

It contains every possible finite word:

$$
a,\ b,\ ab,\ a^{-1},\ aba^{-1}b^{-1},\dots
$$

with only the required group laws:

* identity
* inverses
* associativity

No extra equations.

---

## Formal definition

A free group on a set $X$ is a pair:

$$
(F(X),i)
$$

where:

$$
i:X\rightarrow F(X)
$$

and for every group $G$ and function:

$$
f:X\rightarrow G
$$

there exists a unique homomorphism:

$$
\bar f:F(X)\rightarrow G
$$

such that:

$$
\bar f\circ i=f
$$

Diagram:

$$
\begin{array}{ccc}
X&\xrightarrow{i}&F(X)\
&\searrow_f&\downarrow^{\exists !\bar f}\
&&G
\end{array}
$$

The important part:

$$
\boxed{\exists!}
$$

There is exactly one extension of the generator assignment.

---

# Example 1: Free group on one generator

Let:

$$
X={a}
$$

Then:

$$
F(X)={...,a^{-2},a^{-1},e,a,a^2,...}
$$

This is:

$$
F({a})\cong\mathbb Z
$$

because:

$$
a^n\leftrightarrow n
$$

So:

$$
\boxed{\text{free group on one generator is infinite cyclic}}
$$

---

# Example 2: Free group on two generators

Let:

$$
X={a,b}
$$

Then:

$$
F(a,b)
$$

contains:

$$
ab
$$

and:

$$
ba
$$

as different elements.

Why?

Because we have not imposed:

$$
ab=ba
$$

Therefore:

$$
F(a,b)
$$

is non-abelian.

---

# 2. Quotients of free groups

Most groups are created by:

1. Start free.
2. Add relations.

Example:

Start:

$$
F(a)
$$

Add:

$$
a^5=e
$$

Then:

$$
F(a)/\langle a^5\rangle
$$

gives:

$$
C_5
$$

General form:

$$
\boxed{
G\cong F(X)/N
}
$$

where:

* $X$ = generators
* $N$ = relations

This is called a **presentation**:

$$
G=\langle X\mid R\rangle
$$

Example:

$$
C_5=\langle a\mid a^5=1\rangle
$$

---

# 3. Extensions

Now reverse the viewpoint.

Instead of:

> "How do we generate a group?"

we ask:

> "How do we combine two groups?"

Suppose:

$$
N,Q
$$

An extension asks for a group:

$$
G
$$

such that:

$$
N
$$

is inside:

$$
G
$$

and:

$$
Q
$$

is the remaining quotient.

Formal definition:

An extension of $Q$ by $N$ is a short exact sequence:

$$
1\rightarrow N
\overset{i}{\rightarrow}
G
\overset{\pi}{\rightarrow}
Q
\rightarrow1
$$

---

# 4. Meaning of exactness

Exactness means:

## Injection

$$
i:N\rightarrow G
$$

means:

$$
N\cong i(N)
$$

so $N$ is a subgroup.

---

## Kernel

$$
\ker(\pi)=N
$$

meaning:

the things forgotten by projection are exactly $N$.

---

## Surjection

$$
\pi:G\rightarrow Q
$$

means:

everything in $Q$ is represented.

---

So:

$$
\boxed{
G/N\cong Q
}
$$

---

# Example 1: trivial extension

Take:

$$
N=C_2
$$

and:

$$
Q=C_3
$$

One possible extension:

$$
G=C_2\times C_3
$$

Sequence:

$$
1\rightarrow C_2
\rightarrow C_2\times C_3
\rightarrow C_3
\rightarrow1
$$

Because:

$$
(C_2\times C_3)/C_2\cong C_3
$$

This is a direct product.

---

# Example 2: nontrivial extension: $S_3$

Recall:

$$
|S_3|=6
$$

It contains:

$$
C_3={e,(123),(132)}
$$

which is normal:

$$
C_3\triangleleft S_3
$$

The quotient:

$$
S_3/C_3\cong C_2
$$

Therefore:

$$
1
\rightarrow C_3
\rightarrow S_3
\rightarrow C_2
\rightarrow1
$$

So:

$$
S_3
$$

is an extension of:

$$
C_2
$$

by:

$$
C_3
$$

---

# 5. Why $S_3$ is not $C_3\times C_2$

Because the extension has an action:

$$
C_2\rightarrow Aut(C_3)
$$

Now:

$$
Aut(C_3)\cong C_2
$$

The nontrivial element sends:

$$
x\mapsto x^{-1}
$$

So:

$$
S_3\cong C_3\rtimes C_2
$$

The $C_2$ flips the $C_3$.

---

# 6. Category theory compression

Free group:

$$
\boxed{
\text{left adjoint to the forgetful functor}
}
$$

There is a functor:

$$
U:\mathbf{Grp}\rightarrow\mathbf{Set}
$$

which forgets multiplication.

Free group is:

$$
F:\mathbf{Set}\rightarrow\mathbf{Grp}
$$

with:

$$
F\dashv U
$$

Meaning:

$$
\operatorname{Hom}*{Grp}(F(X),G)
\cong
\operatorname{Hom}*{Set}(X,U(G))
$$

---

Extensions:

They classify ways to glue objects.

Categorically:

$$
\mathrm{Ext}(Q,N)
$$

is the space of possible extensions.

For abelian groups:

$$
\mathrm{Ext}^1(Q,N)
$$

is a derived functor measuring failure of splitting.

---

# 7. The finite group construction pipeline

The categorical construction is:

$$
\text{Set of generators}
$$

↓

free group:

$$
F(X)
$$

↓

impose relations:

$$
F(X)/N
$$

↓

decompose:

$$
1\rightarrow N\rightarrow G\rightarrow Q\rightarrow1
$$

↓

understand interaction:

$$
Q\rightarrow Aut(N)
$$

↓

reconstruct:

$$
N\rtimes Q
$$

The deepest compression:

$$
\boxed{
\text{Free groups create possibilities; extensions describe compositions.}
}
$$
