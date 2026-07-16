Let's completely flatten a **poset** and then a **lattice** into **pure ZFC notation**. This is exactly the exercise of asking: *what does a lattice literally look like when everything is just sets?*

---

# Example 1: A finite set

Let

$$
X={1,2}.
$$

Using ZFC's von Neumann naturals,

$$
0=\varnothing
$$

$$
1={\varnothing}
$$

$$
2={\varnothing,{\varnothing}}.
$$

So

$$
X=
\left{
{\varnothing},
{\varnothing,{\varnothing}}
\right}.
$$

---

# Example 2: Its power set

The carrier of our lattice is

$$
L=\mathcal P(X).
$$

Explicitly,

$$
L=
{
\varnothing,
{1},
{2},
{1,2}
}.
$$

Replacing (1,2) by their actual ZFC definitions,

$$
L=
\left{
\varnothing,
\left{{\varnothing}\right},
\left{{\varnothing,{\varnothing}}\right},
X
\right}.
$$

Already, this is "just a set."

---

# Example 3: Ordered pairs

The order relation is a subset of

$$
L\times L.
$$

But

$$
(a,b)
=====

{{a},{a,b}}
$$

(Kuratowski).

Example:

$$
(\varnothing,{1})
=================

{
{\varnothing},
{\varnothing,{1}}
}.
$$

This is literally a set.

---

# Example 4: The order relation

The order is

$$
R=\subseteq.
$$

As a set,

$$
R=
{
(\varnothing,\varnothing),
(\varnothing,{1}),
(\varnothing,{2}),
(\varnothing,X),
({1},{1}),
({1},X),
({2},{2}),
({2},X),
(X,X)
}.
$$

Notice

$$
R\subseteq L\times L.
$$

Nothing mysterious happened.

A relation **is literally just a set of ordered pairs.**

---

# Example 5: The poset

A poset is simply

$$
(L,R)
$$

where

* (L) is a set,
* (R\subseteq L\times L),
* (R) satisfies

$$
\forall x\in L,;(x,x)\in R
$$

(reflexivity),

$$
\forall x,y\in L,;
(x,y)\in R\land(y,x)\in R
\rightarrow
x=y
$$

(antisymmetry),

and

$$
\forall x,y,z\in L,;
(x,y)\in R
\land
(y,z)\in R
\rightarrow
(x,z)\in R
$$

(transitivity).

That's the **entire ZFC definition** of a partially ordered set.

---

# Example 6: Where does the lattice property appear?

A lattice adds another first-order property.

For every

$$
a,b\in L
$$

there exists an element

$$
m\in L
$$

such that

$$
m\subseteq a
$$

and

$$
m\subseteq b,
$$

and whenever

$$
c\subseteq a
$$

and

$$
c\subseteq b,
$$

then

$$
c\subseteq m.
$$

This defines the **greatest lower bound** (meet).

Similarly,

there exists

$$
j\in L
$$

such that

$$
a\subseteq j,
$$

$$
b\subseteq j,
$$

and for every upper bound (u),

$$
j\subseteq u.
$$

This defines the **least upper bound** (join).

---

# The fully flattened picture

Everything reduces to nested sets:

$$
\boxed{
\text{Lattice}
}
$$

is

$$
(L,R)
$$

where

* (L) is a set,
* (R) is a set,
* elements of (R) are Kuratowski pairs,
* Kuratowski pairs are sets,
* the components of those pairs are themselves sets.

Schematically,

$$
\text{Lattice}
;\rightsquigarrow;
{,{{a},{a,b}},\ldots}
$$

with first-order axioms stating that (R) is a partial order and that every pair of elements has a unique meet and join.

---

# Category-theoretic perspective

This flattening illustrates a general pattern:

* **ZFC layer:** everything is encoded as sets.
* **Order-theoretic layer:** we interpret one of those sets as a binary relation (R \subseteq L \times L).
* **Lattice layer:** we impose universal properties on that relation (existence of greatest lower bounds and least upper bounds).
* **Universal algebra layer:** equivalently, we can replace the relation by two operations (\wedge,\vee : L \times L \to L), where each operation itself is a function, and in ZFC a function is again a set of ordered pairs.

The mathematics happens at the order/algebra level; the ZFC encodings provide a uniform foundation in which all of these structures are ultimately represented as sets.
