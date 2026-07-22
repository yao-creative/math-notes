**Intent: formalize the textbook definition of a Hasse diagram as a finite geometric embedding of a finite poset.**

Let

$$
P=(P,\leq)
$$

be a finite partially ordered set.

The Hasse diagram construction is a map from the abstract poset into geometric data in (\mathbb R^2).

---

## 1. Assign a point in (\mathbb R^2) to every element

Choose a function

$$
p:P\to\mathbb R^2.
$$

The point (p(x)) represents the element (x).

For (x\neq y), we require:

$$
p(x)\neq p(y).
$$

Thus (p) is injective:

$$
\boxed{
p:P\hookrightarrow\mathbb R^2.
}
$$

This is the **embedding of the underlying set of the poset** into the plane.

However, it is not necessarily a topological embedding in the interesting topological sense, because (P) is finite and usually given the discrete topology. The important structure here is that (p) preserves the **order geometry**.

---

# 2. The covering relation

For a poset, (x) is covered by (y), written:

$$
x\prec y,
$$

iff:

$$
x<y
$$

and there does not exist (z\in P) such that:

$$
x<z<y.
$$

Formally:

$$
\boxed{
x\prec y
\iff
x\leq y
\land
x\neq y
\land
\neg\exists z\in P;(x<z<y).
}
$$

The Hasse diagram does **not** draw every relation (x<y). It draws only the covering relation.

So define the edge set:

$$
E_P
=
{(x,y)\in P\times P:x\prec y}.
$$

Then the underlying graph of the Hasse diagram is:

$$
G_P=(P,E_P).
$$

This is the **cover graph** of the poset.

---

# 3. Geometric realization of the cover graph

For every covering pair:

$$
x\prec y,
$$

draw a line segment between:

$$
p(x)
\quad\text{and}\quad
p(y).
$$

Formally:

$$
e(x,y)
=

{(1-t)p(x)+tp(y):0\leq t\leq1}.
$$

Thus the entire set of line segments is:

$$
\mathcal E
=
{e(x,y):(x,y)\in E_P}.
$$

So a diagram is essentially the geometric object:

$$
\boxed{
D(P,p)
=

p[P]\cup
\bigcup_{(x,y)\in E_P}e(x,y).
}
$$

The circles are just visual markers around the points (p(x)).

---

# 4. The vertical order condition

The text says that if:

$$
x<y,
$$

then (p(x)) must be lower than (p(y)).

Let:

$$
\operatorname{pr}_2:\mathbb R^2\to\mathbb R
$$

be the second-coordinate projection:

$$
\operatorname{pr}_2(a,b)=b.
$$

Then the condition is:

$$
\boxed{
x<y
\implies
\operatorname{pr}_2(p(x))
<
\operatorname{pr}_2(p(y)).
}
$$

Equivalently, if:

$$
p(x)=(a_x,b_x),
$$

then:

$$
x<y\implies b_x<b_y.
$$

So the second coordinate is a **strict order-preserving map**:

$$
\operatorname{pr}_2\circ p:
(P,<)\to(\mathbb R,<).
$$

This is possible because every finite poset admits a linear extension.

That is the key structural fact behind the construction.

---

# 5. Why the vertical coordinate always exists

Because (P) is finite, we can choose a linear extension:

$$
x_1,x_2,\ldots,x_n
$$

such that:

$$
x_i<x_j
\implies
i<j.
$$

Then define:

$$
p(x_i)=(a_i,i)
$$

for arbitrary horizontal coordinates (a_i), chosen later.

Therefore:

$$
x<y
\implies
\operatorname{pr}_2(p(x))
<
\operatorname{pr}_2(p(y)).
$$

So the vertical placement is essentially an embedding of the poset into a total order:

$$
\boxed{
P\hookrightarrow\mathbb N\hookrightarrow\mathbb R.
}
$$

The horizontal coordinates are then chosen to prevent unwanted intersections.

---

# 6. The non-intersection condition

The passage's condition (c) is essentially:

$$
e(x,y)
$$

should not intersect the circle representing an unrelated element (z), except at its intended endpoints.

If (C_z) is the circle centered at (p(z)), then:

$$
\boxed{
z\notin{x,y}
\implies
C_z\cap e(x,y)=\varnothing.
}
$$

Depending on the exact textbook convention, one may also require distinct edges not to intersect except at common endpoints:

$$
e(x,y)\cap e(u,v)
\subseteq
{p(x),p(y)}\cap{p(u),p(v)}.
$$

The important point is that the drawing should not create **geometric intersections that falsely suggest order relations**.

---

# The clean set-theoretic formulation

A Hasse diagram of (P) can therefore be represented by a tuple:

$$
\boxed{
D=
(P,\leq,p,\mathcal E)
}
$$

where:

### 1. (P) is finite

$$
|P|<\infty.
$$

### 2. (p) embeds the elements into the plane

$$
p:P\hookrightarrow\mathbb R^2.
$$

### 3. The vertical coordinate respects the order

$$
x<y
\implies
\pi_2(p(x))<\pi_2(p(y)).
$$

### 4. Edges correspond exactly to covers

$$
\mathcal{E}
=
\left\{
\{p(x), p(y)\} : x \prec y
\right\}
$$

### 5. The geometric realization avoids spurious intersections

For every (x\prec y), the segment joining (p(x)) and (p(y)) does not pass through any unrelated vertex circle.

---

## The deeper abstraction

The construction has two layers:

$$
\boxed{
\text{poset}
\longrightarrow
\text{cover graph}
\longrightarrow
\text{geometric embedding in }\mathbb R^2.
}
$$

More formally:

$$
(P,\leq)
\longmapsto
(P,\prec)
\longmapsto
\left(
p:P\hookrightarrow\mathbb R^2,
;
{e(x,y):x\prec y}
\right).
$$

The first step is an **order-theoretic reduction**:

$$
\leq
\quad\rightsquigarrow\quad
\prec.
$$

The second is a **geometric representation problem**.

The important distinction is:

> The Hasse diagram is not an embedding of the poset's order relation directly into (\mathbb R^2). It is an embedding of the **cover graph**, subject to a vertical coordinate that is monotone with respect to the original partial order.

That is why two different diagrams can represent the same poset: the underlying abstract structure

$$
(P,\leq)
$$

is fixed, while the geometric embedding:

$$
p:P\hookrightarrow\mathbb R^2
$$

is not unique.
