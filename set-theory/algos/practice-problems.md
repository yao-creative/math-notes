The technical pattern you are practicing is:

$$
\textbf{Representation change}:\quad X \xrightarrow{P} X'
$$

where the original query:

$$
R:X\rightarrow Y
$$

becomes:

$$
R=Q\circ P
$$

and (Q) is cheap because (X') exposes an algebraic structure (usually a monoid/group).

Here are practice problems organized by the underlying algebraic idea.

---

# Level 1 — Prefix sums (additive group)

## 1. Range Sum Query — Immutable

**Pattern:** prefix sum

Given:

$$
a=[a_0,a_1,\dots,a_n]
$$

Answer:

$$
\sum_{i=l}^{r}a_i
$$

Constraints:

* (n=10^5)
* (q=10^5)

Goal:

Construct:

$$
P(i)=\sum_{k=0}^{i-1}a_k
$$

then:

$$
\text{answer}=P(r+1)-P(l)
$$

---

## 2. Find Pivot Index

Given an array, find index (i) where:

$$
\sum_{k=0}^{i-1}a_k
===================

\sum_{k=i+1}^{n-1}a_k
$$

Hint:

Total:

$$
T=\sum_{k=0}^{n-1}a_k
$$

Need:

$$
L_i=T-L_i-a_i
$$

---

## 3. Product of Array Except Self

Replace addition with multiplication.

Need:

$$
answer_i=\prod_{j\neq i}a_j
$$

The trick:

prefix:

$$
P_i=\prod_{j<i}a_j
$$

suffix:

$$
S_i=\prod_{j>i}a_j
$$

Then:

$$
answer_i=P_iS_i
$$

---

# Level 2 — Prefix sums + hashing (quotienting)

## 4. Subarray Sum Equals K

Find number of subarrays:

$$
\sum_{i=l}^{r}a_i=k
$$

Transform:

$$
P(r+1)-P(l)=k
$$

Rearrange:

$$
P(l)=P(r+1)-k
$$

So store equivalence classes:

$$
{i\mid P(i)=x}
$$

Data structure:

Hash map.

---

## 5. Longest Subarray With Sum K

Find maximum:

$$
r-l+1
$$

such that:

$$
P(r+1)-P(l)=k
$$

Equivalent:

$$
P(l)=P(r+1)-k
$$

Store earliest representative.

This introduces:

$$
\text{equivalence relation}
$$

$$
i\sim j
\iff
P(i)=P(j)
$$

---

## 6. Continuous Subarray Sum

Find if:

$$
\exists l,r:
\sum_{i=l}^{r}a_i
\equiv0\pmod{k}
$$

Transform:

$$
P(r)\equiv P(l)\pmod{k}
$$

Now your equivalence classes are:

$$
[x]_k={y\mid y\equiv x\pmod{k}}
$$

This is quotient group:

$$
\mathbb Z/k\mathbb Z
$$

---

# Level 3 — Prefix XOR (group over (GF(2)))

## 7. XOR Queries

Given:

$$
a=[1,0,1,1]
$$

Answer:

$$
a_l\oplus\dots\oplus a_r
$$

Use:

$$
P(r)\oplus P(l-1)
$$

because:

$$
x\oplus x=0
$$

---

## 8. Count Subarrays With XOR K

Same structure:

Need:

$$
P(r)\oplus P(l)=k
$$

Rearrange:

$$
P(l)=P(r)\oplus k
$$

Same hash-map idea as sum.

---

# Level 4 — 2D prefix sums

## 9. Matrix Region Sum Query

Given:

$$
A\in\mathbb Z^{m\times n}
$$

Answer rectangle:

$$
(x_1,y_1)
\rightarrow
(x_2,y_2)
$$

Build:

$$
P(i,j)=
\sum_{x<i,y<j}A(x,y)
$$

Then:

$$
\begin{aligned}
query=&P(x_2,y_2)
-P(x_1,y_2)\
&-P(x_2,y_1)
+P(x_1,y_1)
\end{aligned}
$$

This is inclusion-exclusion.

---

# Level 5 — Difference arrays (inverse direction)

Here you reverse the trick.

Instead of answering:

$$
\text{range query}
$$

you perform:

$$
\text{range updates}
$$

Example:

Add (v) to:

$$
[l,r]
$$

Store:

$$
diff[l]+=v
$$

$$
diff[r+1]-=v
$$

Then recover:

$$
A_i=\sum_{k=0}^{i}diff[k]
$$

You are applying the inverse of prefix sum.

---

# Level 6 — Advanced: monoid generalization

## 10. Range Minimum Query

Try to apply prefix minimum:

$$
P(i)=\min(a_0,...,a_i)
$$

Why does:

$$
range(l,r)
\neq
P(r)-P(l)
$$

fail?

Answer:

Minimum has no inverse.

Need:

* sparse table
* segment tree

---

## 11. Segment Tree Range Sum

Implement:

$$
([l,r],+)
$$

The node stores:

$$
(\text{interval},\text{sum})
$$

The merge:

$$
(a,+)\circ(b,+)
$$

forms a monoid.

---

## 12. Fenwick Tree

Implement dynamic prefix sums:

Operations:

1. update:

$$
a_i+=x
$$

2. query:

$$
\sum_{k=0}^{i}a_k
$$

This is prefix sum where the underlying representation supports incremental updates.

---

# Meta-practice question

For every problem, ask:

### 1. What is the expensive query?

$$
R(x)
$$

### 2. What representation exposes the hidden algebra?

$$
P:X\rightarrow X'
$$

### 3. What operation becomes easy?

$$
Q(X')
$$

### 4. What algebra is underneath?

* group?
* monoid?
* lattice?
* semiring?

The progression is:

$$
\text{Prefix Sum}
\rightarrow
\text{Hashing}
\rightarrow
\text{XOR}
\rightarrow
\text{2D Inclusion}
\rightarrow
\text{Difference Arrays}
\rightarrow
\text{Fenwick/Segment Tree}
$$

These are essentially different manifestations of the same idea: **find a representation where the required computation becomes composition/inversion in an algebraic structure.**
