This is a much deeper question. The answer is **yes**: the order-theoretic part transfers to code, but **not as execution order**. It transfers as **refinement order**, **information order**, **capability order**, or **effect order**. The isomorphism preserves these structures.

Let's build it from first principles.

---

## 1. The set-theoretic bijection forgets semantics

As sets,

$$
(P \times Q \to R) \cong (P \to (Q \to R))
$$

just says there is a reversible encoding.

Nothing about code changes.

---

## 2. The order-theoretic bijection preserves semantic structure

Suppose the codomain $R$ is ordered.

Then the function spaces inherit pointwise order:

$$
f \le g \iff \forall p, q,\; f(p,q) \le_R g(p,q).
$$

The currying map satisfies

$$
f \le g \iff \operatorname{curry}(f) \le \operatorname{curry}(g).
$$

So it is an **order isomorphism**.

Now ask:

> What does $\le_R$ mean in software?

That depends on the semantics.

---

# Interpretation 1: Capability lattice (Security)

Suppose

$$
Guest \le User \le Admin.
$$

Then

$$
authorize_1 \le authorize_2
$$

means

> every permission granted by authorize₁ is also granted by authorize₂.

Currying preserves this.

Whether you write

```rust id="wtk40j"
authorize(user, resource)
```

or

```rust id="8ncn0w"
authorize(user)(resource)
```

the privilege ordering is unchanged.

This is why refactoring to closures doesn't accidentally change security semantics.

---

# Interpretation 2: Information order

Suppose outputs become "more informative."

Example:

```text id="3feq1i"
Unknown
≤
Partial
≤
Verified
```

Now

```rust id="xj42f8"
lookup(user, id)
```

returns information.

Currying:

```rust id="pd14m9"
lookup(user)(id)
```

preserves

$$
Unknown < Partial < Verified.
$$

The closure doesn't change the refinement ordering.

---

# Interpretation 3: Effect systems

Suppose effects are ordered.

Example:

$$
Pure < Read < Write < Network.
$$

Then

```rust id="f34p4e"
f(a,b)
```

has some effect.

Currying gives

```rust id="qpj4zg"
f(a)(b)
```

The order of effects is identical.

The closure doesn't create extra authority.

---

# Interpretation 4: Preconditions

This one is extremely important.

Suppose

$$
VerifiedUser \le AuthenticatedUser \le Anonymous.
$$

Here "$\le$" means

> satisfies stronger invariants.

Then

```rust id="4rr29t"
transfer(user, money)
```

can be rewritten

```rust id="8lhqlr"
transfer(user)(money)
```

The first call discharges a proof obligation.

The returned function has a refined type.

In dependent-type languages this literally becomes

$$
Authenticated \to (Money \to Result).
$$

The order now represents **logical refinement**.

---

# Interpretation 5: Domain theory (the deepest one)

This is where the order-theoretic version really originates.

Functions are ordered by approximation.

$$
f \le g
$$

means

> $g$ computes at least as much information as $f$.

Example:

Partial computation:

```text id="u0rmgp"
⊥
```

Eventually:

```text id="8fud9k"
42
```

Then

$$
\bot < 42.
$$

Currying preserves approximation.

This matters in:

* lazy evaluation
* fixed-point semantics
* recursive interpreters
* denotational semantics

This is one of the main reasons order theory appears in programming language semantics.

---

# Category-theoretically

Here's the categorical picture.

In **Set**,

$$
\mathrm{Hom}(P \times Q, R) \cong \mathrm{Hom}(P, R^Q)
$$

is just a bijection.

In **Poset** or **CPO** (complete partial orders),

the hom-sets themselves are ordered.

Now the theorem is

$$
[P \times Q, R] \cong [P, [Q, R]]
$$

as **posets**.

Meaning:

* currying preserves comparisons,
* preserves refinement,
* preserves approximation,
* preserves monotonicity.

Not merely cardinality.

---

# Practical software interpretation

The transfer isn't:

> function A executes before function B.

Instead it is:

> every semantic ordering on computations survives the refactoring.

If

```rust id="9d5m8k"
unsafe ≤ safe
```

then after currying

```rust id="7zjlwm"
unsafe_closure ≤ safe_closure
```

If

```text id="0ldc4l"
less permissions ≤ more permissions
```

then

```text id="pph8eb"
curried less permissions ≤ curried more permissions
```

If

```text id="z4yzcf"
less information ≤ more information
```

then

```text id="2jlwmj"
curried less information ≤ curried more information.
```

---

This is why order-theoretic currying is much more than a syntactic trick. It says that **currying is an isomorphism in a category enriched by an order**, so every semantic notion represented by that order—refinement, capability, information, approximation, effects, or logical strength—is preserved by the transformation. That preservation is what makes currying a semantics-preserving refactoring rather than merely a change in how arguments are passed.