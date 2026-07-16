The prefix sum trick is a good example because it has **three layers**:

1. **Set theory:** define the objects and functions precisely.
2. **Algebra:** exploit the group structure of addition.
3. **Category theory:** view prefix sums as a change of representation via a homomorphism/natural transformation-like construction.

---

## 1. Set-theoretic formulation

Let an array of length $n$ be a function:

$$
a:\{0,1,\dots,n-1\}\rightarrow \mathbb{Z}
$$

The set of all arrays is:

$$
\mathbb{Z}^{n}
$$

Define the range-sum query:

$$
R:\mathbb{Z}^{n}\times\{0,\dots,n-1\}\times\{0,\dots,n-1\}
\rightarrow \mathbb{Z}
$$

where:

$$
R(a,l,r)=\sum_{k=l}^{r}a(k)
$$

Naively:

$$
R(a,l,r)
$$

requires $O(r-l+1)$ operations.

---

## 2. Construct a new representation

Define the prefix sum function:

$$
P:\mathbb{Z}^{n}\rightarrow \mathbb{Z}^{n+1}
$$

where:

$$
P(a)(0)=0
$$

and:

$$
P(a)(i+1)=P(a)(i)+a(i)
$$

Explicitly:

$$
P(a)=
(0,
a_0,
a_0+a_1,
\dots,
\sum_{k=0}^{n-1}a_k)
$$

This is a transformation:

$$
\boxed{
P:\text{Arrays}\rightarrow\text{Prefix Arrays}
}
$$

---

## 3. The key theorem

The original query:

$$
R(a,l,r)
$$

is equivalent to:

$$
P(a)(r+1)-P(a)(l)
$$

Proof:

By definition:

$$
P(a)(r+1)=\sum_{k=0}^{r}a(k)
$$

and:

$$
P(a)(l)=\sum_{k=0}^{l-1}a(k)
$$

Therefore:

$$
P(a)(r+1)-P(a)(l)
=
\left(\sum_{k=0}^{r}a(k)\right)
-
\left(\sum_{k=0}^{l-1}a(k)\right)
$$

The terms cancel:

$$
\sum_{k=l}^{r}a(k)
$$

So:

$$
\boxed{
R = Q\circ P
}
$$

where:

$$
Q(p,l,r)=p(r+1)-p(l)
$$

The expensive operation has been replaced by a constant-time operation.

### Same proof in set-builder notation

Let the index sets be:

$$
I_r := \{k\in\mathbb{Z}\mid 0\le k\le r\},
\qquad
I_{l-1} := \{k\in\mathbb{Z}\mid 0\le k\le l-1\},
\qquad
S_{l,r} := \{k\in\mathbb{Z}\mid l\le k\le r\}.
$$

Then we have a disjoint union:

$$
I_r = I_{l-1}\;\dot{\cup}\;S_{l,r}.
$$

Applying $\sum_{k\in(\cdot)} a(k)$ to both sides gives:

$$
\sum_{k\in I_r} a(k)
=
\sum_{k\in I_{l-1}} a(k)
+
\sum_{k\in S_{l,r}} a(k),
$$

so:

$$
\sum_{k\in S_{l,r}} a(k)
=
\sum_{k\in I_r} a(k)
-
\sum_{k\in I_{l-1}} a(k).
$$

Finally, by definition of prefix sums,
$$
P(a)(r+1)=\sum_{k\in I_r} a(k)
\quad\text{and}\quad
P(a)(l)=\sum_{k\in I_{l-1}} a(k),
$$
hence
$$
R(a,l,r)=\sum_{k\in S_{l,r}} a(k)=P(a)(r+1)-P(a)(l).
$$

---

# Category theory view

## 1. Arrays as objects

Consider a category:

$$
\mathbf{Arr}
$$

where:

* Objects are array spaces.
* Morphisms are array transformations.

An array is an object:

$$
A=\mathbb{Z}^{n}
$$

The prefix transformation is a morphism:

$$
P:A\rightarrow A'
$$

where:

$$
A'=\mathbb{Z}^{n+1}
$$

---

## 2. Why subtraction works: group structure

The integers form a group:

$$
(\mathbb{Z},+,0,-)
$$

The prefix operation uses the monoid:

$$
(\mathbb{Z},+,0)
$$

but the query inversion uses the group inverse:

$$
x+(-y)
$$

The fundamental equation is:

$$
\sum_{k=0}^{r} a(k)
=
\sum_{k=0}^{l-1} a(k)
+
\sum_{k=l}^{r} a(k)
$$

so:

$$
\sum_{k=l}^{r} a(k)
=
\left(\sum_{k=0}^{r} a(k)\right)
-
\left(\sum_{k=0}^{l-1} a(k)\right)
$$

The trick works because the accumulated structure is invertible.

---

## 3. Homomorphism view

Define accumulation:

$$
\operatorname{fold}_+:
\mathbb{Z}^{n}\rightarrow\mathbb{Z}
$$

by:

$$
(a_0,\dots,a_{n-1})
\mapsto
a_0+\cdots+a_{n-1}
$$

The prefix map preserves the algebraic operation:

$$
P(a\oplus b)=P(a)\oplus P(b)
$$

when $\oplus$ is pointwise addition.

So (P) is a homomorphism between additive structures:

$$
(\mathbb{Z}^{n},+)
\rightarrow
(\mathbb{Z}^{n+1},+)
$$

---

## 4. General pattern

Prefix sums are an instance of a broader categorical pattern:

$$
\begin{array}{c}
\text{Hard query}\\
R:X\rightarrow Y\\
\downarrow\\
\text{Change representation}\\
P:X\rightarrow X'\\
\downarrow\\
\text{Easy query}\\
Q:X'\rightarrow Y
\end{array}
$$

such that:

$$
\boxed{
R=Q\circ P
}
$$

You replace one morphism with a composition through a better object.

---

## 5. Why this generalizes

The prefix trick works for any **group-valued fold**:

### Sum

$$
(\mathbb{Z},+)
$$

inverse exists:

$$
a-b
$$

### XOR

$$
({0,1}^n,\oplus)
$$

inverse:

$$
x\oplus x=0
$$

So:

$$
range=a_r\oplus a_{l-1}
$$

### Minimum?

No inverse:

$$
\min(a,b)
$$

has no operation:

$$
x\circ y=z
$$

that lets you recover $x$.

Therefore prefix minimum cannot answer arbitrary range minimum queries; you need another structure (segment tree, sparse table).

---

The deep algorithmic principle is:

$$
\boxed{
\text{Precompute a morphism into a representation where the query becomes an inverse operation}
}
$$

Prefix sums are not just an optimization trick; they are a concrete instance of **changing coordinates so that a computation becomes algebraically trivial**.
