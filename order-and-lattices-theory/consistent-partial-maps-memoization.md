**Technical intent: you are asking whether the “consistent family of partial maps” viewpoint gives a useful abstraction for memoization. Yes—this is almost exactly the right formal model.**

A memoization table is naturally a partial map:

$$
M : X \rightharpoonup Y
$$

where

* $X$ = possible inputs,
* $Y$ = computed results,
* $x\in\operatorname{dom}(M)$ means the result for $x$ has been cached.

A cache insertion is the extension:

$$
M' = M\cup{(x,y)}.
$$

This is valid iff

$$
x\notin\operatorname{dom}(M)
\quad\lor\quad
M(x)=y.
$$

So the fundamental cache invariant is exactly **consistency**:

$$
\boxed{
\forall (x,y),(x,y')\in M,\quad y=y'
}
$$

---

## 1. Memoization is incremental construction of a partial function

Suppose a recursive algorithm computes

$$
f:X\to Y.
$$

Initially:

$$
M_0=\varnothing.
$$

Then each computation produces a partial map:

$$
{(x,f(x))}.
$$

The memoization process is:

$$
M_0
\subseteq
M_1
\subseteq
M_2
\subseteq
\cdots
$$

where

$$
M_{n+1}=M_n\cup{(x,f(x))}.
$$

The invariant is:

$$
M_n\subseteq f.
$$

This means every cached result is correct.

Thus the cache is not merely "a dictionary"; mathematically it is an **increasing chain of partial approximations to a total function**.

---

## 2. Why consistency matters for concurrent memoization

Suppose two threads compute the same input:

$$
T_1:x\mapsto y_1
$$

and

$$
T_2:x\mapsto y_2.
$$

For a deterministic function, correctness requires:

$$
y_1=y_2.
$$

Then the two local partial maps

$$
M_1={(x,y_1)}
$$

and

$$
M_2={(x,y_2)}
$$

are consistent exactly when:

$$
y_1=y_2.
$$

Therefore a memoization table can safely merge independently computed results if the computation is deterministic:

$$
M_1\cup M_2
$$

remains a function.

This gives a useful design distinction:

### Deterministic memoization

$$
x\mapsto y
$$

has a unique result.

Then independently computed partial caches can be merged.

### Nondeterministic memoization

$$
x\mapsto y_1
\quad\text{and}\quad
x\mapsto y_2
$$

may legitimately differ.

Then the cache cannot be modeled as an ordinary partial function. You need something like:

$$
X\to\mathcal P(Y)
$$

or a versioned/temporal map.

---

## 3. Memoization tables as embeddings

Here the embedding idea becomes useful.

A cache is an embedding of the **known portion of the computation** into the result space:

$$
M:D\hookrightarrow Y
$$

where

$$
D\subseteq X.
$$

Because $M$ is a partial function, it does not necessarily cover all inputs:

$$
D\neq X.
$$

But if you want to call it an **embedding**, you additionally require injectivity:

$$
M(x_1)=M(x_2)\implies x_1=x_2.
$$

That is usually **not required for memoization**.

For example:

$$
f(1)=0,\qquad f(2)=0.
$$

The memo table

$$
{(1,0),(2,0)}
$$

is perfectly valid, but not injective.

So the more accurate abstraction is:

$$
\boxed{
\text{memoization table}
=

\text{consistent partial map}
}
$$

not necessarily an embedding.

---

## 4. The really useful connection: merging caches

Suppose you have two memoization tables:

$$
M_1:D_1\rightharpoonup Y
$$

and

$$
M_2:D_2\rightharpoonup Y.
$$

You want:

$$
M_1\cup M_2.
$$

This is valid iff:

$$
\forall x\in D_1\cap D_2,\quad
M_1(x)=M_2(x).
$$

So cache merging is literally the same operation as **gluing consistent partial maps**.

This appears in:

* parallel dynamic programming,
* distributed computation,
* build caches,
* compiler memoization,
* content-addressed storage,
* ML feature caches,
* incremental computation,
* replicated state.

The merge operation is a **partial operation**:

$$
M_1\sqcup M_2
$$

defined only when

$$
M_1\sim M_2
$$

where

$$
M_1\sim M_2
\iff
\forall x\in\operatorname{dom}(M_1)\cap\operatorname{dom}(M_2),
\quad
M_1(x)=M_2(x).
$$

When defined:

$$
M_1\sqcup M_2=M_1\cup M_2.
$$

This is algebraically interesting: consistent partial maps form a structure under compatible union.

---

## 5. The software design consequence

The key invariant for a memoization table is:

$$
\boxed{
\text{same semantic input}
\longrightarrow
\text{same cached result}
}
$$

But this immediately reveals a subtle issue:

$$
\text{same key}
\not\Rightarrow
\text{same semantic input}.
$$

For example, if a function depends on hidden state:

```python
f(x) = x + global_counter
```

then caching only by `x` is invalid.

The true function is actually:

$$
f(x,s)
$$

where $s$ is the relevant state.

So the correct memoization map is:

$$
M:(X\times S)\rightharpoonup Y.
$$

This is perhaps the most useful practical interpretation:

> **Memoization is valid exactly when the cache key is an adequate quotient of the function's input state.**

If two states are identified by the same cache key but produce different results, then the quotient is too aggressive and consistency fails.

For example, if

$$
k:X\to K
$$

is your cache-key function, then memoization requires:

$$
k(x_1)=k(x_2)
\implies
f(x_1)=f(x_2).
$$

Equivalently, $f$ must factor through the key:

$$
\begin{CD}
X @>{f}>> Y \\
@V{k}VV @AA{\bar f}A \\
K @>{g}>> Y
\end{CD}
$$

meaning:

$$
f=\bar f\circ k.
$$

So the cache key induces an equivalence relation:

$$
x_1\sim x_2
\iff
k(x_1)=k(x_2),
$$

and memoization is sound only if $f$ is constant on every equivalence class.

That is the deeper connection:

$$
\boxed{
\text{memoization correctness}
=

\text{consistent partial-map construction}
+
\text{a sound quotient of inputs}
}
$$
