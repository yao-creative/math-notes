There are actually **three theorems** that are often conflated in textbooks. They build on one another. Let (S) be a semigroup and (a\in S).

---

## Theorem 1 (Eventual Periodicity of Powers)

This is the fundamental theorem.

> **Theorem.** Let (S) be a **finite semigroup**. Then for every (a\in S), there exist integers
>
> $$
> m\ge1,\qquad p\ge1
> $$
>
> such that
>
> $$
> a^{m}=a^{m+p}.
> $$

Equivalently,

$$
a^{m+k}=a^{m+k+p}
\qquad
\forall k\ge0.
$$

The smallest such (m) is called the **index** of (a), and the smallest such (p) is the **period** of (a).

### Proof

Consider

$$
a,a^2,a^3,\ldots
$$

Since (S) is finite, two powers must coincide:

$$
a^i=a^j,
\qquad
i<j.
$$

Let

$$
m=i,\qquad p=j-i.
$$

Multiplying both sides on the right by (a^k) gives

$$
a^{m+k}=a^{m+p+k}.
$$

So the sequence is eventually periodic.

---

# Theorem 2 (The Periodic Part is a Group)

Once periodicity exists, the cycle has much more structure.

> **Theorem.**
>
> Let
>
> $$
> C={a^m,a^{m+1},\ldots,a^{m+p-1}}.
> $$
>
> Then (C) is a cyclic group under multiplication.

This is a surprisingly deep fact.

The transient part eventually disappears, and the remaining cycle is not merely periodic—it is actually a finite cyclic group.

---

# Theorem 3 (Existence of an Idempotent)

This is the theorem your book is using when it writes "(a^{mp}) is idempotent."

> **Theorem.**
>
> Let (S) be finite. Then for every (a\in S), there exists an integer (N\ge1) such that
>
> $$
> a^N
> $$
>
> is idempotent:
>
> $$
> (a^N)^2=a^N.
> $$

A convenient choice is

$$
N=mp,
$$

where (m) is the index and (p) is the period.

### Proof

Since

$$
a^{m+k}=a^{m+k+p}
\qquad(k\ge0),
$$

any exponents (r,s\ge m) satisfying

$$
r\equiv s\pmod p
$$

give

$$
a^r=a^s.
$$

Now let

$$
N=mp.
$$

Then

* (N\ge m),
* (2N\ge m),
* (2N\equiv N\pmod p).

Therefore

$$
a^{2N}=a^N,
$$

so

$$
(a^N)^2=a^N.
$$

Hence (a^N) is idempotent.

---

# The logical dependency

The theorems form a strict chain:

```mermaid
flowchart LR
A["Finite semigroup"] --> B["Pigeonhole principle"]
B --> C["There exist m,p with a^m = a^(m+p)"]
C --> D["Eventually periodic powers"]
D --> E["Periodic part is a cyclic group"]
E --> F["Some power is idempotent"]
```

The crucial point is that **everything starts with finiteness**. Without finiteness, Theorem 1 is false, and consequently Theorems 2 and 3 can also fail. For example, in the infinite semigroup ((\mathbb{N},+)), the powers (1,2,3,\dots) never repeat, so there is no index, no period, no periodic kernel, and no nontrivial idempotent arising from repeated powers.

This chain is one of the foundational structural results in finite semigroup theory and underlies Green's relations, Rees theory, Krohn–Rhodes theory, and algorithms such as Floyd's cycle detection.
