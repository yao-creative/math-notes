In ZFC, a **lattice** can be defined axiomatically as:

A lattice is a structure:

$$
(L,\le)
$$

where:

1. (L) is a set.

2. (\le) is a binary relation:

$$
\le \subseteq L\times L
$$

such that:

### Partial order axioms

Reflexive:

$$
\forall a\in L,\quad a\le a
$$

Antisymmetric:

$$
\forall a,b\in L,\quad
(a\le b\land b\le a)\Rightarrow a=b
$$

Transitive:

$$
\forall a,b,c\in L,\quad
(a\le b\land b\le c)\Rightarrow a\le c
$$

### Lattice axioms

For every (a,b\in L):

There exists a join (a\vee b\in L):

$$
a\le a\vee b,\quad b\le a\vee b
$$

and

$$
\forall c\in L,\quad
(a\le c\land b\le c)\Rightarrow a\vee b\le c
$$

There exists a meet (a\wedge b\in L):

$$
a\wedge b\le a,\quad a\wedge b\le b
$$

and

$$
\forall c\in L,\quad
(c\le a\land c\le b)\Rightarrow c\le a\wedge b
$$

Equivalently, algebraically:

$$
(L,\vee,\wedge)
$$

where

$$
\vee,\wedge:L\times L\rightarrow L
$$

satisfy the lattice identities.
