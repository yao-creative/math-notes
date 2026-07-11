The phrase **"navigate a preorder"** has a precise categorical meaning:

> You are moving through the morphisms of the category induced by the preorder.

The "space" where paths live is not a geometric space by default; it is the **nerve (simplicial space) of the preorder category**.

Let's unpack this.

---

## 1. A preorder as a category

A preorder is a set (P) with relation:

$$
\leq \subseteq P\times P
$$

satisfying:

### Reflexive

$$
x\leq x
$$

### Transitive

$$
x\leq y,\ y\leq z
\implies x\leq z
$$

Unlike a partial order, we allow:

$$
x\leq y,\ y\leq x
$$

without requiring:

$$
x=y
$$

Turn it into a category:

[
\mathcal{P}
]

where:

Objects:

$$
Ob(\mathcal P)=P
$$

Morphisms:

$$
\operatorname{Hom}(x,y)=
\begin{cases}
{*}&x\leq y\
\emptyset&\text{otherwise}
\end{cases}
$$

So:

$$
x\leq y
\iff
x\rightarrow y
$$

---

## 2. What does "navigate" mean?

Navigation means choosing a sequence of composable morphisms:

$$
x_0\rightarrow x_1\rightarrow x_2\rightarrow\cdots\rightarrow x_n
$$

which corresponds to:

$$
x_0\leq x_1\leq x_2\leq\cdots\leq x_n
$$

This is a **chain** in the preorder.

Example:

Suppose:

$$
A\leq B\leq C
$$

Navigation:

$$
A\rightarrow B\rightarrow C
$$

You are not moving through physical space; you are traversing **ordered information/state refinement**.

---

## 3. Where do paths live?

There are several interpretations.

### Level 1: The preorder category itself

The simplest answer:

[
\boxed{\text{Paths live inside the morphisms of }\mathcal P}
]

A path is a compositional chain:

$$
x_0\to x_1\to...\to x_n
$$

---

### Level 2: The nerve of the category

Category theory converts a category into a simplicial set called its **nerve**:

$$
N(\mathcal P)
$$

The (n)-simplices are:

$$
N_n(\mathcal P)
===============

{x_0\rightarrow x_1\rightarrow\cdots\rightarrow x_n}
$$

So:

* (0)-simplices = objects
* (1)-simplices = comparisons
* (2)-simplices = composable triangles
* (n)-simplices = chains of length (n)

The "space" is:

$$
|N(\mathcal P)|
$$

called the **geometric realization** of the preorder.

This creates an actual topological space.

---

## 4. Example: a simple preorder

Take:

$$
a\leq b\leq c
$$

Category:

$$
a\rightarrow b\rightarrow c
$$

The nerve contains:

### Points

$$
a,b,c
$$

### Edges

$$
a\rightarrow b
$$

$$
b\rightarrow c
$$

$$
a\rightarrow c
$$

### Triangle

Because:

$$
a\rightarrow b\rightarrow c
$$

composition fills:

$$
a\rightarrow c
$$

Geometrically it becomes a filled simplex:

$$
\Delta^2
$$

---

## 5. Connection to Hasse diagrams

A Hasse diagram removes transitive edges.

So instead of showing:

$$
a\rightarrow b,\quad b\rightarrow c,\quad a\rightarrow c
$$

it only shows:

$$
a\rightarrow b\rightarrow c
$$

The Hasse diagram is the **covering relation**, not the full categorical structure.

The category contains all paths:

$$
a\rightarrow c
$$

because composition creates it.

---

## 6. Intuition: preorder as a state space

A useful mental model:

A preorder defines a **reachability space**.

Examples:

### Version control

$$
v_1\leq v_2
$$

means:

"version (v_2) contains all information of (v_1)."

A path:

$$
v_1\rightarrow v_2\rightarrow v_3
$$

is a history.

---

### Program states

$$
S_0\leq S_1
$$

means:

"state (S_1) has more information/refinement than (S_0)."

A path is an execution trace.

---

### Type systems

$$
A\leq B
$$

means:

"A can be used where B is expected."

Navigation is moving through subtype relationships.

---

## 7. The categorical hierarchy

The relationship is:

$$
\text{Preorder}
\subset
\text{Category}
$$

because:

[
\boxed{
\text{preorder = category where every Hom-set has at most one element}
}
]

Then:

$$
\text{Preorder}
\rightarrow
\text{Nerve}
\rightarrow
\text{Simplicial set}
\rightarrow
\text{Topological space}
$$

So the answer to:

> "What space do paths on a preorder live in?"

is:

**Algebraically:** they live in the preorder category's morphisms.

**Combinatorially:** they live in the nerve (N(P)).

**Geometrically:** they live in the realization (|N(P)|), a topological space built from all chains.
