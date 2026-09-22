Labeling each code line with its **PTS judgment form** $\Gamma \vdash e : A$ and its grammar correspondent ($x$, $\Pi x^{t}.t'$, $\lambda x^{t}.t'$, $t_1\,t_2$) inline as a comment, so the syntactic surface and the calculus term are visually adjacent rather than separated into prose.

## Variable — $x^{A}$, judgment $\Gamma, x{:}A \vdash x : A$

```typescript
// x^{number} : number   |   Γ, x:number ⊢ x : number
const x: number = 5;
```
```python
# x^{int} : int   |   Γ, x:int ⊢ x : int
x: int = 5
```
```ocaml
(* x^{int} : int   |   Γ, x:int ⊢ x : int *)
let x : int = 5
```
```rust
// x^{&i32} : &i32   |   Γ, x:&i32 ⊢ x : &i32   (borrowed, never owned)
fn use_x(x: &i32) -> i32 { *x }
```

## Product — $\Pi x^{A}.B$, judgment $\Gamma \vdash \Pi x^{A}.B : \mathrm{Type}$

Non-dependent case (the only case these languages express): $x \notin \mathrm{FV}(B)$, so $\Pi x^{A}.B$ degenerates to $B^A = A \to B$.

```typescript
// Π x^{number} . string  ≡  number → string   |   Γ ⊢ Arrow : Type
type Arrow = (x: number) => string;
```
```python
# Π x^{int} . str  ≡  int → str   |   Γ ⊢ Arrow : Type
from typing import Callable
Arrow = Callable[[int], str]
```
```ocaml
(* Π x^{int} . string  ≡  int → string   |   Γ ⊢ arrow : Type *)
type arrow = int -> string
```
```rust
// Π x^{i32} . String  ≡  i32 → String   |   Γ ⊢ Arrow : Type
type Arrow<'a> = &'a dyn Fn(i32) -> String;
```

## Abstraction — $\lambda x^{A}.t$, judgment $\dfrac{\Gamma, x{:}A \vdash t : B}{\Gamma \vdash \lambda x^{A}.t : \Pi x^{A}.B}$

```typescript
// λ x^{number} . x.toString()   |   Γ, x:number ⊢ x.toString() : string  ⇒  Γ ⊢ f : Arrow
const f: Arrow = (x) => x.toString();
```
```python
# λ x^{int} . str(x)   |   Γ, x:int ⊢ str(x) : str  ⇒  Γ ⊢ f : Arrow
f: Arrow = lambda x: str(x)
```
```ocaml
(* λ x^{int} . string_of_int x   |   Γ, x:int ⊢ string_of_int x : string  ⇒  Γ ⊢ f : arrow *)
let f : arrow = fun x -> string_of_int x
```
```rust
// λ x^{&i32} . x.to_string()   |   Γ, x:&i32 ⊢ x.to_string() : String  ⇒  Γ ⊢ f : Arrow
// closure borrows x; no internal state is owned or initialized
let f = |x: &i32| -> String { x.to_string() };
```

## Application — $t_1\,t_2$, judgment $\dfrac{\Gamma \vdash t_1 : \Pi x^{A}.B \quad \Gamma \vdash t_2 : A}{\Gamma \vdash t_1\,t_2 : B[x := t_2]}$

Since these are all non-dependent, $B[x := t_2] = B$ (the substitution is vacuous — $x \notin \mathrm{FV}(B)$).

```typescript
// f^{Π x^{number}.string}  applied to  5^{number}   |   Γ ⊢ f(5) : string
f(5)
```
```python
# f^{Π x^{int}.str}  applied to  5^{int}   |   Γ ⊢ f(5) : str
f(5)
```
```ocaml
(* f^{Π x^{int}.string}  applied to  5^{int}   |   Γ ⊢ f 5 : string *)
f 5
```
```rust
// f^{Π x^{&i32}.String}  applied to  &5^{&i32}   |   Γ ⊢ f(&5) : String
// argument borrowed, not moved — f never takes ownership of caller state
f(&5)
```

---

One thing worth flagging structurally: in every non-Rust case above, $t_2$ (the argument) is passed **by value** into the substitution slot $x := t_2$ — there's no distinction in the calculus between "owning" and "borrowing" $t_2$. Rust's `&` is precisely a *refinement* of the application rule that the vanilla PTS doesn't have a primitive for: it's tracking an affine/linear-ish usage discipline on top of $\Pi$-elimination, not a different elimination rule per se. If you want, I can formalize that refinement explicitly — e.g. as an indexed judgment $\Gamma \vdash t_2 : A \mid \rho$ where $\rho \in \{\mathsf{own}, \mathsf{borrow}, \mathsf{mut}\}$ — since that's the exact axis your Rust preference is constraining.