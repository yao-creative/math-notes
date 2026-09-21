Framing this **hierarchically**: I'll first fix the invariant structure — what sorts/variable/Π/λ/application *mean* independent of syntax — then instantiate that structure across the four languages, since your target languages are simply-typed (no term-level sorts, no genuine dependent Π), so most of the "collapsing" that happens is itself informative.

## The invariant structure (category-theoretic frame)

In a **cartesian closed category** $\mathcal{C}$, the non-dependent fragment of your grammar corresponds to:

$$
\Pi x^A.B \;\rightsquigarrow\; B^A \quad(\text{exponential object, when } x \notin \mathrm{FV}(B))
$$

$$
\lambda x^A.t \;\rightsquigarrow\; \Lambda(t) : C \to B^A \quad \text{(currying / transpose of } t: C\times A \to B\text{)}
$$

$$
t_1\,t_2 \;\rightsquigarrow\; \mathrm{eval}\circ\langle t_1,t_2\rangle, \quad \mathrm{eval}: B^A \times A \to B
$$

None of TypeScript, Python, OCaml, or Rust have **sorts as terms** — they don't let you write `x : Type : Kind` inside expressions the way a PTS does. So "sorts" degenerate to the *type/kind* stratification each language happens to expose, and I'll flag exactly where that stratification stops.

---

## Sorts

| Lang | What plays the role of $s$ | Where it stops |
|---|---|---|
| TypeScript | `type` (the universe of types) has no reflective term-level access; but `*` ≈ `unknown`/`any` as a "top sort" informally | No kinds beyond `*`; generics are not terms |
| Python | Nothing formal — `type` is a runtime *value*, so `type(int)` gives you `type`, and `type(type)` is `type` itself (a genuine fixed point, unusual for a sort!) | No static kind system at all |
| OCaml | `Type` as a kind exists only in the module/functor layer (`module type`), not the core term language | Core language: one sort, `Type`, unreflected |
| Rust | Kinds are implicit (`Sized`, `?Sized` distinguish "sorts" of types); no term-level sort | No user-visible sort hierarchy |

## Variable

$$
\Gamma, x:A \vdash x : A
$$

```typescript
// TS
const x: number = 5;
```
```python
# Python — no static annotation required, but can be given
x: int = 5
```
```ocaml
(* OCaml *)
let x : int = 5
```
```rust
// Rust — borrowed, never owned by a "functional component"
fn use_x(x: &i32) -> i32 { *x }
```

## Product ($\Pi x^A.B$ → non-dependent exponential $B^A$, i.e. function type)

```typescript
type Arrow = (x: number) => string;
```
```python
from typing import Callable
Arrow = Callable[[int], str]
```
```ocaml
(* OCaml's arrow type IS the exponential, directly *)
type arrow = int -> string
```
```rust
// Rust: function-pointer/closure trait as exponential object B^A
type Arrow<'a> = &'a dyn Fn(i32) -> String;
```

## Abstraction ($\lambda x^A.t$)

```typescript
const f: Arrow = (x) => x.toString();
```
```python
f: Arrow = lambda x: str(x)
```
```ocaml
let f : arrow = fun x -> string_of_int x
```
```rust
// closure borrows its environment, never owns mutable state internally
let f = |x: &i32| -> String { x.to_string() };
```

## Application ($t_1\,t_2$, i.e. $\mathrm{eval}\circ\langle t_1,t_2\rangle$)

```typescript
f(5) // eval(f, 5)
```
```python
f(5)
```
```ocaml
f 5
```
```rust
f(&5) // borrow passed in, closure never takes ownership of caller state
```

---

**One structural note worth flagging explicitly**, since it bears on your Rust constraint: none of these four languages give you a $\Pi$ that can *depend* on its argument's value at the type level (only OCaml's module functors and, partially, TypeScript's conditional/generic types approximate it). If you want a fifth column showing what genuine dependent $\Pi$ looks like (e.g. in Idris or Agda) as a contrast case, I can add that — it would make the "where the analogy breaks" boundary sharper.