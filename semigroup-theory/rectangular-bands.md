You are connecting several ideas that are genuinely related, but there are two different structures being mixed:

1. **rectangular bands as ordered/lattice-like objects**
2. **the logical "middle disappears" phenomenon (interpolation / existential elimination / cut-like ideas)**

Let's separate them.

---

## 1. Does a rectangular band form a preorder lattice?

A rectangular band itself is **not naturally a lattice**.

A lattice requires two operations:

$$
\vee,\wedge:S\times S\rightarrow S
$$

satisfying:

* idempotence
* commutativity
* associativity
* absorption:

$$
x\wedge(x\vee y)=x
$$

A rectangular band only has:

$$
xyx=x
$$

and

$$
xyz=xz
$$

The multiplication is generally **not commutative**:

$$
(i,j)(k,l)=(i,l)
$$

but:

$$
(k,l)(i,j)=(k,j)
$$

so usually:

$$
xy\neq yx
$$

Therefore it is not a lattice.

---

However, there is a close connection.

A **band** (any idempotent semigroup) induces a preorder.

Define:

$$
x\preceq y
$$

if:

$$
xy=x
$$

(or variants depending on left/right conventions).

Interpretation:

> (x) is absorbed by (y).

For a rectangular band:

Take:

$$
x=(i,j),\quad y=(k,l)
$$

Then:

$$
xy=(i,l)
$$

For:

$$
xy=x
$$

we need:

$$
(i,l)=(i,j)
$$

so:

$$
l=j
$$

Meaning:

$$
(i,j)\preceq(k,j)
$$

Only the **right coordinate matters**.

The preorder collapses into equivalence classes.

---

## 2. The hidden lattice-like structure is Green's relations

This is why rectangular bands appear in semigroup theory.

Green relations define a hierarchy:

$$
\mathcal{L},\mathcal{R},\mathcal{J},\mathcal{H}
$$

For:

$$
S=I\times J
$$

we have:

### Same left coordinate

$$
(i,j)\mathcal{R}(i,k)
$$

because they generate the same right ideals.

### Same right coordinate

$$
(i,j)\mathcal{L}(k,j)
$$

because they generate the same left ideals.

The whole semigroup is one:

$$
\mathcal{D}\text{-class}
$$

which is why it is called "rectangular".

The picture is more like a grid:

$$
\begin{matrix}
(i_1,j_1)&(i_1,j_2)&(i_1,j_3)\\
(i_2,j_1)&(i_2,j_2)&(i_2,j_3)
\end{matrix}
$$

Moving horizontally changes one Green class.

Moving vertically changes another.

---

# 3. The "middle disappears" connection

Your second intuition is actually very interesting.

The law:

$$
xyz=xz
$$

says:

$$
\boxed{\text{the middle term is irrelevant}}
$$

This resembles several logical ideas.

---

## A. Existential quantification

In logic:

$$
\exists y.\ P(x,y,z)
$$

means:

"There exists some middle witness (y), but we do not care which one."

The variable is hidden.

Example:

$$
\exists y(x<y<z)
$$

The middle point disappears from the external description.

Similarly:

$$
xyz=xz
$$

says:

$$
y
$$

is an internal witness that leaves no observable trace.

---

## B. Category theory: composition through an object

Composition:

$$
X\xrightarrow{f}Y\xrightarrow{g}Z
$$

has middle object:

$$
Y
$$

The category remembers:

$$
g\circ f
$$

but not the internal pointwise details of (Y).

The rectangular band law is an extreme version:

$$
X\rightarrow Y\rightarrow Z
$$

where the middle morphism contributes no information.

---

## C. Relational algebra

A database join:

$$
R(A,B)\Join S(B,C)
$$

has a middle attribute (B).

Projection removes it:

$$
\pi_{A,C}(R\Join S)
$$

The intermediate column disappears.

This is almost exactly:

$$
xyz=xz
$$

The middle relation participates but is forgotten.

---

# 4. Why this matters for software design

This gives a design heuristic:

When you see:

$$
A\rightarrow B\rightarrow C
$$

ask:

> "Is (B) essential state, or only an intermediate representation?"

If it disappears observationally, you can refactor.

Examples:

### Compiler:

$$
AST \rightarrow IR \rightarrow MachineCode
$$

If:

$$
AST \rightarrow IR_1 \rightarrow MachineCode
$$

and

$$
AST \rightarrow IR_2 \rightarrow MachineCode
$$

produce identical output, IR is an internal witness.

---

### API pipeline:

```
Request
   |
 validation
   |
 authorization
   |
 execution
```

If authorization leaves no trace except allowing execution, it is a hidden middle morphism.

---

# 5. The deeper categorical analogy

A rectangular band is like a **lossy composition law**.

Ordinary composition:

$$
f\circ g\circ h
$$

keeps all structure.

Rectangular band:

$$
xyz=xz
$$

is saying:

$$
\text{composition factors through a projection}
$$

The middle object is quotiented away.

So your intuition is close:

$$
\boxed{
\text{rectangular band}
\approx
\text{a system where composition forgets the middle}
}
$$

The closest mathematical relatives are not lattices, but:

* quotient constructions
* projections
* Green relation posets
* existential hiding
* database projection
* categorical composition with forgotten intermediates

The "lattice" feeling comes from the induced **preorder of absorption**, but the fundamental structure is a **factorization/projection structure**, not a lattice.
