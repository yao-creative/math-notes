**Intent: conceptual mastery check — testing whether you can distinguish universe stratification, self-reference, and quantification.**

Here’s a progression. Try answering **without looking anything up**.

### Level 1 — Core distinction

1. Suppose

$$
\mathcal U_0 : \mathcal U_1.
$$

and

$$
A:\mathcal U_0.
$$

If we construct

$$
B = \prod_{X:\mathcal U_0} F(X),
$$

why is putting $B$ in $\mathcal U_1$ naturally **predicative**?

2. What specifically would change if we instead had

$$
\mathcal U_0:\mathcal U_0?
$$

Don't answer "Russell's paradox." Explain the **circular dependency**.

3. Is this statement true?

> "Predicativity means that nothing can refer to itself."

Give a counterexample if false.

---

### Level 2 — Quantification

4. Consider:

```text
A : Type₀
B : A → Type₀
C : (x : A) → B x
```

Is there anything impredicative here? Why?

5. Now consider:

```text
P : Prop
Q : (P : Prop) → P → P
```

What is the crucial question you should ask to determine whether this is impredicative?

6. Suppose a system permits:

$$
P:\mathsf{Prop}
$$

and

$$
\forall Q:\mathsf{Prop},\, Q\to Q:\mathsf{Prop}.
$$

Why does this **not immediately imply**

$$
\mathsf{Prop}:\mathsf{Prop}?
$$

This one is particularly important.

---

### Level 3 — Russell intuition

7. In naïve set theory:

$$
R=\{x\mid x\notin x\}.
$$

What is the precise structural mistake?

Is it:

A. self-reference
B. negation
C. unrestricted comprehension
D. foundation being absent

Pick the best answer and explain why.

8. Suppose instead we have:

$$
R=\{x\in A\mid x\notin x\}.
$$

Why is this fundamentally different?

---

### Level 4 — Type-theoretic reasoning

9. Suppose:

$$
\mathcal U_0:\mathcal U_1
$$

and

$$
F:\mathcal U_0\to\mathcal U_0.
$$

Consider:

$$
P=\prod_{X:\mathcal U_0}F(X).
$$

Where would you expect $P$ to live?

Now ask:

> Why does the fact that $F(X)$ lives in $\mathcal U_0$ **not automatically imply** that $P$ lives in $\mathcal U_0$?

10. Imagine someone says:

> "If `Type₀` contains every type I'm interested in, then quantifying over `Type₀` should produce another `Type₀`."

What's wrong with this reasoning?

---

### Level 5 — The killer question

Consider the following hypothetical language:

```text
U : U

forall X : U, F(X) : U
```

Someone tells you:

> "This is safe because there is no recursive function and therefore no computational infinite loop."

**Why is that response missing the point?**

Try to explain the issue entirely in terms of **sets/types, quantification, and levels**, without mentioning computation.

---

### Final synthesis

If you can answer these three precisely, you've probably got the concept:

**A.**

$$
\mathcal U_0:\mathcal U_1
$$

Why does this break the circularity?

**B.**

$$
\mathcal U_0:\mathcal U_0
$$

Why is this qualitatively different?

**C.**

Why can a theory be **impredicative but still consistent**, as with an impredicative proposition universe?

The distinction I would especially watch for in your answers is:

$$
\boxed{
\text{self-reference}
\neq
\text{impredicativity}
\neq
\text{inconsistency}
}
$$

They interact, but they are three different concepts.
