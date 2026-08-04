# Monad Diagrams

Here are the standard category-theoretic diagrams for a monad.

## 1. Monad data

A monad consists of an endofunctor and two natural transformations:

```mermaid
graph LR
    C["𝒞"] -- "T" --> C2["𝒞"]

    eta["η : Id ⇒ T"]
    mu["μ : T∘T ⇒ T"]
```

Equivalently,

$$T:\mathcal C\to\mathcal C$$

$$\eta:\mathrm{Id}_{\mathcal C}\Rightarrow T$$

$$\mu:T^2\Rightarrow T.$$

---

## 2. Unit (η)

For every object $X$,

```mermaid
graph LR
    X["X"] -->|"ηₓ"| TX["T(X)"]
```

$$\eta_X:X\rightarrow T(X).$$

---

## 3. Multiplication (μ)

```mermaid
graph LR
    TTX["T(T(X))"] -->|"μₓ"| TX["T(X)"]
```

$$\mu_X:T(T(X))\rightarrow T(X).$$

---

## 4. Left identity law

The following diagram commutes.

```mermaid
graph LR
    TX1["T(X)"] -->|"η_{T(X)}"| TTX["T(T(X))"]
    TTX -->|"μ_X"| TX2["T(X)"]
    TX1 -->|"id"| TX2
```

meaning

$$\mu_X\circ\eta_{T(X)}=\operatorname{id}_{T(X)}.$$

---

## 5. Right identity law

```mermaid
graph LR
    TX1["T(X)"] -->|"T(η_X)"| TTX["T(T(X))"]
    TTX -->|"μ_X"| TX2["T(X)"]
    TX1 -->|"id"| TX2
```

meaning

$$\mu_X\circ T(\eta_X)=\operatorname{id}_{T(X)}.$$

---

## 6. Associativity

This is the key coherence condition.

```mermaid
graph TD
    TTTX["T(T(T(X)))"]

    TTTX -->|"T(μₓ)"| TTX1["T(T(X))"]
    TTX1 -->|"μₓ"| TX1["T(X)"]

    TTTX -->|"μ_{T(X)}"| TTX2["T(T(X))"]
    TTX2 -->|"μₓ"| TX2["T(X)"]

    TX1 --- TX2
```

which states

$$\mu_X\circ T(\mu_X)=\mu_X\circ\mu_{T(X)}.$$

---

## 7. Kleisli composition

Given

$$f:A\to T(B),\qquad g:B\to T(C),$$

their composition is

```mermaid
graph LR
    A -->|"f"| TB["T(B)"]
    TB -->|"T(g)"| TTC["T(T(C))"]
    TTC -->|"μ"| TC["T(C)"]
```

or algebraically,

$$g\star f=\mu_C\circ T(g)\circ f.$$

This diagram shows why a monad provides a notion of composition for **effectful morphisms**: $T(g)$ lifts the second computation into the effectful context, and $\mu$ collapses the resulting nested effect back into a single layer.