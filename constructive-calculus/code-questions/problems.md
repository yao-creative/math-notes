Structuring this as a **graded problem set indexed by which grammar production the problem exercises**, so each problem isolates one constructor of the calculus before problems compose them. Each problem states its **judgment-form goal** first (what you must derive/produce), then the Python task, then what to self-check against.

---

## Block A — Variable ($x^A$)

**Goal:** produce $\Gamma, x{:}A \vdash x : A$ for a nontrivial $A$.

**A1.** Write a Python function `identity_check` that takes a parameter `x: list[int]` and simply returns `x`. Annotate the type. What is $\Gamma$ at the point you return `x`, and what judgment licenses the `return x` line?

**A2.** *(negative case)* Write a snippet where you reference a variable `y` **outside** the scope in which it was bound (e.g. use a variable defined only inside a function, at module level). Identify precisely which set-theoretic operation in $\mathrm{FV}$ this violates — i.e. why $y \notin \mathrm{FV}(\text{module body})$.

---

## Block B — Product ($\Pi x^{A}.B$, realized as `Callable[[A], B]`)

**Goal:** state a type $\Pi x^{A}.B$ before implementing anything.

**B1.** Declare (annotation only, no body yet) a type alias `Transformer = Callable[[float], float]`. Write out by hand the judgment $\Gamma \vdash \mathrm{Transformer} : \Pi x^{\text{float}}.\text{float}$ — i.e. confirm $x \notin \mathrm{FV}(\text{float})$, so it's really $A \to B$ and not a dependent product.

**B2.** *(forces dependency)* Construct a case where the **codomain genuinely depends on the argument's value**, which `Callable` cannot express directly. Concretely: write a function `describe(n: int) -> ...` where the return type would be `Literal["even"]` when `n` is even and `Literal["odd"]` when `n` is odd. Since Python's type system can't express this as one $\Pi$-type, explain in one sentence why this *would* require a genuine dependent $\Pi x^{\text{int}}.T(x)$ rather than a fixed exponential $B^A$.

---

## Block C — Abstraction ($\lambda x^{A}.t$)

**Goal:** given a stated $\Pi$-type from Block B, inhabit it via $\lambda$.

**C1.** Inhabit `Transformer` from B1: write `f: Transformer = lambda x: x ** 2 + 1`. Write the full typing derivation:
$$
\frac{\Gamma, x{:}\text{float} \vdash x^{**}2+1 : \text{float}}{\Gamma \vdash \lambda x^{\text{float}}.(x^{**}2+1) : \Pi x^{\text{float}}.\text{float}}
$$
Identify which subterm of the body corresponds to two nested **applications** (there are two — find both).

**C2.** *(closure = free variable capture)* Write
```python
def make_adder(k: int) -> Callable[[int], int]:
    return lambda x: x + k
```
Compute $\mathrm{FV}(\lambda x^{\text{int}}.(x+k))$ by hand. Which variable is free and which is bound? Explain why `make_adder` returning this lambda is exactly the phenomenon that forces $\mathrm{FV}$, rather than mere alpha-renaming, to matter operationally (i.e. `k` must be resolved from the *enclosing* $\Gamma$, not substituted at definition time).

---

## Block D — Application ($t_1\,t_2$)

**Goal:** derive the result type via the elimination rule, then execute it and confirm.

**D1.** Given `f` from C1, write `f(3.0)`. State the instance of the elimination rule:
$$
\frac{\Gamma \vdash f : \Pi x^{\text{float}}.\text{float} \qquad \Gamma \vdash 3.0 : \text{float}}{\Gamma \vdash f(3.0) : \text{float}[x := 3.0] = \text{float}}
$$
Note explicitly that the substitution in the conclusion is **vacuous** here — say why, in terms of $\mathrm{FV}(\text{float})$.

**D2.** *(iterated application / currying)* Write a two-argument curried function:
```python
def curried_add(x: int) -> Callable[[int], int]:
    return lambda y: x + y
```
Write `curried_add(2)(3)` and derive its type via **two successive applications** of the elimination rule (first application produces an intermediate $\Pi$-type, second consumes it). State both intermediate judgments explicitly.

---

## Block E — Composition / synthesis (all five productions together)

**E1.** Implement categorical composition as a term:
```python
def compose(g: Callable[[int], str], f: Callable[[int], int]) -> Callable[[int], str]:
    return lambda x: g(f(x))
```
Label **every subterm** in `lambda x: g(f(x))` with its grammar production ($x$, application, application again — note `f(x)` and `g(...)` are two distinct application nodes) and its type, the way we did in the previous message's format.

**E2.** *(sorts, made visible via `type()`)* Using `type(int)`, `type(type)`, and `type(5)`, explore Python's one place where the sort/kind stratification becomes an actual runtime term rather than an erased annotation. State which of these calls returns a fixed point ($s$ such that $\mathrm{type}(s) = s$), and explain why no such fixed point can exist in a *stratified* PTS (i.e. why $\mathrm{Type} : \mathrm{Type}$ is famously inconsistent — Girard's paradox) even though Python permits it at runtime with no logical consequence, since Python's `type` isn't a typing judgment in the PTS sense at all.

---

Want me to give **model solutions with full derivations** for these now, or would you rather submit your attempts first so I can grade against the judgment forms?