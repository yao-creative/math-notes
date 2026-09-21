**Intent: formal proof of the cardinality isomorphism**

$$
2^{\mathbb N}\cong[0,1).
$$

The cleanest route is to define the binary-expansion map explicitly, identify exactly where it fails to be injective, and then fix that ambiguity canonically.

Let

$$
2=\{0,1\},
\qquad
2^{\mathbb N}=\{a\mid a:\mathbb N\to 2\}.
$$

Thus an element \(a\in2^{\mathbb N}\) is an infinite binary sequence

$$
a=(a_0,a_1,a_2,\ldots).
$$

---

## 1. Define the candidate map

Define

$$
F:2^{\mathbb N}\to[0,1]
$$

by

$$
\boxed{
F(a)=\sum_{n=0}^{\infty}\frac{a_n}{2^{n+1}}.
}
$$

This is well-defined because

$$
0\leq
\sum_{n=0}^{\infty}\frac{a_n}{2^{n+1}}
\leq
\sum_{n=0}^{\infty}\frac1{2^{n+1}}
=1.
$$

So indeed

$$
F(a)\in[0,1].
$$

The intuition is simply

$$
(a_0,a_1,a_2,\ldots)
\mapsto
0.a_0a_1a_2\ldots_2.
$$

---

# 2. Why \(F\) is not quite injective

There is the familiar identity

$$
0.1000\ldots_2
=
0.0111\ldots_2.
$$

In sequence notation:

$$
F(1,0,0,0,\ldots)
=
F(0,1,1,1,\ldots).
$$

More generally, whenever a sequence eventually contains

$$
1,0,0,0,\ldots
$$

at some position, it has an alternative representation where that \(1\) is decreased to \(0\) and all subsequent digits become \(1\).

So \(F\) is **surjective onto \([0,1]\)** but not injective.

This is precisely the issue you were identifying earlier.

---

# 3. Restrict the domain canonically

Define

$$
C
=
\left\{
a\in2^{\mathbb N}
\mid
a\text{ is not eventually }1
\right\}.
$$

Formally,

$$
a\in C
\iff
\forall N\in\mathbb N,\,
\exists n\geq N
\text{ such that }a_n=0.
$$

In words:

> There are infinitely many \(0\)'s.

Now restrict \(F\):

$$
F|_C:C\to[0,1].
$$

But we still have a problem:

$$
(1,1,1,\ldots)\notin C,
$$

so \(1\) has no representation in \(C\).

Indeed,

$$
F(1,1,1,\ldots)=1.
$$

Thus

$$
F|_C:C\to[0,1)
$$

is the natural candidate.

And this is what we will prove is a bijection.

---

# 4. Proposition

Define

$$
F:C\to[0,1)
$$

by

$$
F(a)=\sum_{n=0}^{\infty}\frac{a_n}{2^{n+1}}.
$$

Then

$$
\boxed{F:C\xrightarrow{\cong}[0,1)}
$$

is a bijection.

We'll prove:

1. well-definedness;
2. injectivity;
3. surjectivity.

---

# 5. Injectivity

Suppose

$$
F(a)=F(b)
$$

for

$$
a,b\in C.
$$

We want to prove

$$
a=b.
$$

Assume for contradiction that

$$
a\neq b.
$$

Since \(a,b:\mathbb N\to\{0,1\}\), there must exist some \(k\in\mathbb N\) such that

$$
a_k\neq b_k.
$$

Because \(\mathbb N\) is well ordered, there is a **least** such \(k\).

Therefore

$$
a_n=b_n
\qquad\forall n<k.
$$

Since the digits are binary, there are only two possibilities.

Without loss of generality, suppose

$$
a_k=0,\qquad b_k=1.
$$

Then

$$
F(b)-F(a)
=
\frac1{2^{k+1}}
+
\sum_{n=k+1}^{\infty}
\frac{b_n-a_n}{2^{n+1}}.
$$

Since

$$
b_n-a_n\geq -1,
$$

we have

$$
F(b)-F(a)
\geq
\frac1{2^{k+1}}
-
\sum_{n=k+1}^{\infty}\frac1{2^{n+1}}.
$$

Now

$$
\sum_{n=k+1}^{\infty}\frac1{2^{n+1}}
=
\frac1{2^{k+1}}.
$$

So merely from this we obtain

$$
F(b)-F(a)\geq0.
$$

The equality case is the interesting part.

Equality can occur **only if**

$$
b_n-a_n=-1
\qquad\forall n>k.
$$

Since the digits are \(0\) or \(1\), this means

$$
b_n=0,\qquad a_n=1
\qquad\forall n>k.
$$

Therefore

$$
a=(\cdots,0,1,1,1,1,\ldots)
$$

after position \(k\), meaning that \(a\) is eventually \(1\).

But

$$
a\in C
$$

by assumption.

Contradiction.

Therefore

$$
F(a)\neq F(b)
$$

whenever

$$
a\neq b.
$$

Hence

$$
\boxed{F\text{ is injective}.}
$$

This is the rigorous version of the **first differing digit argument**.

---

# 6. Surjectivity

Now take an arbitrary

$$
x\in[0,1).
$$

We need to construct

$$
a\in C
$$

such that

$$
F(a)=x.
$$

There are several equivalent constructions. The most formal one is to define the digits recursively.

Set

$$
r_0=x.
$$

For each \(n\in\mathbb N\), define

$$
a_n=\lfloor2r_n\rfloor
$$

and

$$
r_{n+1}=2r_n-a_n.
$$

Because

$$
0\leq r_n<1,
$$

we have

$$
0\leq2r_n<2,
$$

so

$$
a_n=\lfloor2r_n\rfloor\in\{0,1\}.
$$

Furthermore,

$$
0\leq r_{n+1}<1.
$$

Thus recursively we obtain

$$
a=(a_0,a_1,a_2,\ldots)\in2^{\mathbb N}.
$$

---

# 7. Show that the digits actually represent \(x\)

From

$$
r_{n+1}=2r_n-a_n
$$

we obtain

$$
r_n=\frac{a_n+r_{n+1}}2.
$$

Apply this repeatedly:

$$
x=r_0
=
\frac{a_0}{2}
+
\frac{r_1}{2}.
$$

Then

$$
r_1
=
\frac{a_1+r_2}{2},
$$

so

$$
x
=
\frac{a_0}{2}
+
\frac{a_1}{2^2}
+
\frac{r_2}{2^2}.
$$

Continuing,

$$
x
=
\sum_{i=0}^{N}
\frac{a_i}{2^{i+1}}
+
\frac{r_{N+1}}{2^{N+1}}.
$$

Since

$$
0\leq r_{N+1}<1,
$$

we have

$$
0\leq
\frac{r_{N+1}}{2^{N+1}}
<
\frac1{2^{N+1}}
\to0.
$$

Therefore, taking the limit,

$$
\boxed{
x=
\sum_{i=0}^{\infty}\frac{a_i}{2^{i+1}}
}.
$$

Thus

$$
F(a)=x.
$$

So every \(x\in[0,1)\) has a binary sequence representation.

---

# 8. But we still need \(a\in C\)

This is the subtle part.

We need to show that the sequence generated above is **not eventually \(1\)**.

Suppose instead that there exists \(N\) such that

$$
a_n=1
\qquad\forall n\geq N.
$$

Then

$$
x
=
\sum_{n=0}^{N-1}\frac{a_n}{2^{n+1}}
+
\sum_{n=N}^{\infty}\frac1{2^{n+1}}.
$$

But

$$
\sum_{n=N}^{\infty}\frac1{2^{n+1}}
=
\frac1{2^N}.
$$

Therefore

$$
x
=
\sum_{n=0}^{N-1}\frac{a_n}{2^{n+1}}
+
\frac1{2^N}.
$$

The first sum is at most

$$
\sum_{n=0}^{N-1}\frac1{2^{n+1}}
=
1-\frac1{2^N}.
$$

Therefore

$$
x\leq1.
$$

That alone doesn't contradict \(x<1\), because equality occurs only when all the preceding \(a_n\)'s are \(1\).

So we need to examine the actual possibilities.

If some \(a_j=0\) for \(j<N\), then

$$
x<1.
$$

That's fine.

But in that case, the generated sequence is still eventually \(1\), meaning it is the **noncanonical** representation.

So the recursive algorithm by itself doesn't guarantee our desired canonical sequence.

This reveals something important:

> **The ordinary binary expansion algorithm gives a representation, but not automatically the canonical one.**

---

# 9. Canonicalize it

For \(x\in[0,1)\), there are two cases.

### Case 1: \(x\) has a terminating binary expansion

Then

$$
x=
\sum_{n=0}^{N}\frac{a_n}{2^{n+1}}
$$

for some finite sequence of digits.

Use the representation

$$
a_0a_1\ldots a_N0000\ldots
$$

rather than

$$
a_0a_1\ldots(a_N-1)1111\ldots.
$$

This representation is **not eventually \(1\)**.

### Case 2: \(x\) does not have a terminating binary expansion

Then its binary expansion is already not eventually \(1\).

Thus every

$$
x\in[0,1)
$$

has at least one representation in \(C\).

And from the injectivity proof, it has **at most one**.

Therefore exactly one.

Hence

$$
\boxed{
C\cong[0,1).
}
$$

---

# 10. One subtle correction to the earlier conceptual picture

There is an even cleaner formulation.

Rather than saying

$$
2^{\mathbb N}\cong[0,1),
$$

directly, the precise statement is

$$
\boxed{
2^{\mathbb N}_{\mathrm{canonical}}
\cong
[0,1)
}
$$

where

$$
2^{\mathbb N}_{\mathrm{canonical}}
=
\{a:\mathbb N\to\{0,1\}:a\text{ is not eventually }1\}.
$$

The full set \(2^{\mathbb N}\) is **not** bijective to \([0,1)\) via the naive binary-expansion map because of the dyadic ambiguity.

However, cardinality-wise,

$$
|C|=|2^{\mathbb N}|,
$$

because the excluded sequences—those eventually equal to \(1\)—form only a countable set.

So eventually you can conclude

$$
\boxed{
|2^{\mathbb N}|=|C|=|[0,1)|.
}
$$

And then, since

$$
[0,1)\subseteq\mathbb R,
$$

you can finish the cardinality proof by constructing the other injection

$$
\mathbb R\hookrightarrow 2^{\mathbb N}
$$

or, more elegantly, by first establishing

$$
\mathbb R\cong(0,1)
$$

with a standard bijection such as

$$
x\mapsto\frac12+\frac1\pi\arctan x,
$$

and then use the inclusion

$$
(0,1)\hookrightarrow\mathbb R.
$$

That gives Cantor–Schröder–Bernstein.

The key proof pattern you're developing is exactly the right one:

$$
\boxed{
\text{representation}
\rightarrow
\text{canonicalization}
\rightarrow
\text{first-difference injectivity}
\rightarrow
\text{recursive construction for surjectivity}
}
$$
