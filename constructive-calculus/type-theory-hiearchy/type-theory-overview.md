**Intent: taxonomy + landscape assessment** — you're asking for the major families of type theory and where there is broad agreement about their roles, rather than a single “best” type theory.

There isn't a single consensus type theory. There is, however, a fairly clear **family tree** of ideas.

### 1. The major families

| Family                                 | Core idea                                                   | Typical systems                        |
| -------------------------------------- | ----------------------------------------------------------- | -------------------------------------- |
| **Simple type theory**                 | Terms have non-dependent types                              | Simply Typed λ-calculus, HOL           |
| **Polymorphic type theory**            | Types can quantify over types                               | System F, Hindley–Milner               |
| **Dependent type theory**              | Types may depend on terms                                   | Martin-Löf TT, CIC, CoC                |
| **Higher-order dependent type theory** | Dependent types + rich universes/impredicativity            | Calculus of Constructions, CIC         |
| **Homotopy type theory**               | Types behave like spaces; equality has higher structure     | HoTT                                   |
| **Univalent type theory**              | Equivalent structures can be identified via equivalence     | HoTT + univalence                      |
| **Cubical type theory**                | Computational treatment of paths/equality                   | Cubical Type Theory                    |
| **Linear type theory**                 | Resources must be used according to linearity               | Linear λ-calculus, Linear Logic        |
| **Substructural type theories**        | Control weakening/contraction/exchange                      | Linear, affine, ordered type systems   |
| **Modal type theory**                  | Types encode modalities such as necessity, stages, locality | Modal TT, guarded TT                   |
| **Intersection / refinement types**    | Types describe properties of terms                          | Intersection types, refinement systems |
| **NuPRL / computational type theory**  | Type theory as a computational foundation                   | NuPRL                                  |

There are also theories designed primarily around **programming-language semantics**, such as effect systems, session types, ownership/borrow systems, and gradual types.

---

## 2. The historical foundational progression

A useful conceptual sequence is:

$$
\text{Simply Typed}
\rightarrow
\text{Polymorphic}
\rightarrow
\text{Dependent}
\rightarrow
\text{Higher-order Dependent}
\rightarrow
\text{Homotopical}
$$

Very roughly:

### Simply typed λ-calculus

You have

$$
\Gamma \vdash t:A
$$

but types don't depend on terms.

For example:

$$
\mathsf{Vec}
$$

isn't naturally able to say “vectors **of length $n$**.”

---

### System F

Now you can quantify over types:

$$
\Lambda A.\;t
$$

and

$$
\forall A.\;A\to A.
$$

This gives **parametric polymorphism**.

Haskell's parametric polymorphism is conceptually descended from this world.

---

### Dependent type theory

Now types can depend on terms:

$$
\mathsf{Vec}(A,n).
$$

So you can express:

$$
\mathsf{Vec}(A,3)
$$

and

$$
\mathsf{Vec}(A,4)
$$

as different types.

This is where **Martin-Löf type theory, CoC, CIC, Agda, Lean, Coq** become particularly relevant.

---

# 3. CoC is one point in this space

The **Calculus of Constructions** is especially important because it combines several ideas:

$$
\boxed{
\text{dependent types}
+
\text{higher-order functions}
+
\text{polymorphism}
+
\text{type universes}
}
$$

CIC—the Calculus of Inductive Constructions—is essentially the richer foundation historically associated with Coq.

Lean uses a closely related dependent type-theoretic foundation, but is not simply “CoC.”

---

# 4. Then HoTT changes what equality means

This is probably the most important conceptual branch if you're coming from set theory.

In ordinary mathematical thinking you tend to have:

$$
a=b
$$

as a proposition.

In intensional type theory, you instead have an **identity type**:

$$
\mathsf{Id}_A(a,b)
$$

or equivalently:

$$
a =_A b.
$$

But now an equality itself is an object/term:

$$
p : a =_A b.
$$

And potentially:

$$
p,q : a=_A b
$$

with

$$
p\neq q.
$$

So equality can have **higher-dimensional structure**.

This leads to the hierarchy:

$$
\text{terms}
\rightarrow
\text{equalities between terms}
\rightarrow
\text{equalities between equalities}
\rightarrow\cdots
$$

That is the conceptual doorway to **homotopy type theory**.

---

# 5. Univalence

HoTT adds the idea that equivalent types can correspond to equal types.

Very roughly:

$$
A\simeq B
\quad\longleftrightarrow\quad
A=B.
$$

More precisely, the univalence axiom says that equivalences between types correspond to paths in the universe.

This is a radically different foundational perspective from naive set theory:

$$
\boxed{\text{structure-preserving equivalence becomes foundationally significant}}
$$

rather than merely being an external relation we happen to define.

---

# 6. Cubical type theory

Cubical type theory is particularly interesting because it tries to make this computational.

Instead of merely **postulating** sophisticated equality principles, cubical systems give computational machinery for paths.

This is important for proof assistants because ideally you want:

$$
\text{proof}
\longrightarrow
\text{program}
$$

with computational behavior rather than merely having a proposition that happens to be true.

Cubical Agda is the most prominent example.

---

# 7. What is actually the consensus?

There are several different kinds of consensus.

### Consensus 1 — dependent type theory is extremely important

There is broad agreement that dependent type theory is a powerful foundation for formal mathematics and verified programming.

The major systems include:

* Coq
* Lean
* Agda
* Idris
* NuPRL

But **no consensus says one of these foundations is mathematically “the correct one.”**

---

### Consensus 2 — Curry–Howard is fundamental

There is extremely strong conceptual consensus around:

$$
\boxed{
\text{propositions}
\leftrightarrow
\text{types}
}
$$

$$
\boxed{
\text{proofs}
\leftrightarrow
\text{terms}
}
$$

$$
\boxed{
\text{proof construction}
\leftrightarrow
\text{program construction}
}
$$

This isn't merely a philosophical slogan; it is an enormous part of modern proof-assistant design.

---

### Consensus 3 — intensional vs extensional equality is a real foundational tradeoff

There isn't agreement that one is universally superior.

**Intensional type theory** prioritizes computational behavior:

$$
\text{equality has computational content}.
$$

**Extensional type theory** makes equality behave more like ordinary mathematical equality.

The tradeoff is essentially:

$$
\text{computational tractability}
\quad\leftrightarrow\quad
\text{extensional mathematical convenience}.
$$

---

### Consensus 4 — HoTT/univalence is important, but not universally adopted

HoTT has become a major foundational program, particularly for formalized mathematics and higher-dimensional mathematics.

But it has **not replaced CIC, MLTT, ZFC, HOL, etc.**

There is no consensus that:

$$
\text{HoTT} > \text{CIC} > \text{ZFC}
$$

or anything analogous.

They're different foundational frameworks.

---

# 8. If your actual question is “what should I learn?”

Given your interest in **set theory → algebra → category theory → type theory**, I would structure the landscape as:

$$
\boxed{
\text{STLC}
\rightarrow
\text{System F}
\rightarrow
\text{Martin-Löf Type Theory}
\rightarrow
\text{Calculus of Constructions}
\rightarrow
\text{CIC}
\rightarrow
\text{HoTT}
\rightarrow
\text{Univalence/Cubical TT}
}
$$

while studying **linear/substructural type theory** as a parallel branch:

$$
\text{λ-calculus}
\rightarrow
\text{Linear Logic}
\rightarrow
\text{Linear Type Theory}
\rightarrow
\text{Resource-aware programming}.
$$

And the really useful correspondence to keep in your head is:

$$
\begin{array}{ccc}
\text{Set theory} &\leftrightarrow& \text{Type theory}\\
\hline
x\in A &\leftrightarrow& x:A\\
A\subseteq B &\leftrightarrow& A\to B\text{ / subtyping, depending on interpretation}\\
A\times B &\leftrightarrow& A\times B\\
\coprod_i A_i &\leftrightarrow& \Sigma_{i:I}A_i\\
\prod_i A_i &\leftrightarrow& \Pi_{i:I}A_i\\
\text{equality} &\leftrightarrow& \mathsf{Id}_A(a,b)
\end{array}
$$

The **$\Pi/\Sigma$ distinction** is especially important: once you understand dependent products and dependent sums deeply, the relationship between ordinary set theory, dependent type theory, category theory, and programming-language type systems becomes much less mysterious.
