Yes — and for recursive functions, the **functor diagram is actually the more fundamental diagram** once you move from “a function calls itself” to **recursive data types and recursion schemes**.

### 1. Start with an ordinary functor

A functor

$$
F:\mathcal C\to\mathcal D
$$

maps:

$$
\boxed{
\text{objects }X\mapsto F(X)
}
$$

and

$$
\boxed{
\text{morphisms }f:X\to Y
\mapsto
F(f):F(X)\to F(Y)
}
$$

with

$$
F(\mathrm{id}_X)=\mathrm{id}_{F(X)}
$$

and

$$
F(g\circ f)=F(g)\circ F(f).
$$

The basic diagram is therefore:

```text
        f
   X ---------> Y
   |             |
 F |             | F
   v             v
  F(X) -------> F(Y)
        F(f)
```

The square commutes:

$$
F(f)\circ F_X = F_Y\circ f
$$

— more precisely, the two paths represent the same morphism whenever the surrounding diagram is appropriately typed.

---

## 2. For recursion, the important functor is a **polynomial/container-like functor**

Take lists.

A list of \(X\)'s satisfies

$$
\operatorname{List}(X)
\cong
1+X\times\operatorname{List}(X).
$$

Define

$$
F(X)=1+X\times X.
$$

Then the recursive list type is conceptually the **fixed point**

$$
\mu F
$$

satisfying

$$
\mu F\cong F(\mu F).
$$

So:

$$
\boxed{
\operatorname{List}(X)
\cong
1+X\times\operatorname{List}(X)
}
$$

The functor diagram becomes:

```text
                    F
        X ----------------------> F(X)
        |                           |
        | f                         | F(f)
        v                           v
        Y ----------------------> F(Y)
                    F
```

For the list functor

$$
F_X(A)=1+X\times A,
$$

a function

$$
f:A\to B
$$

gets lifted to

$$
F_X(f):
1+X\times A
\to
1+X\times B
$$

by

$$
F_X(f)(\ast)=\ast
$$

and

$$
F_X(f)(x,a)=(x,f(a)).
$$

That is exactly what “mapping over the recursive structure” means.

---

# 3. Now the really important recursion diagram

Suppose

$$
\mu F
$$

is the recursive data type.

There is an isomorphism

$$
\mathsf{in}:F(\mu F)\to\mu F.
$$

Then if you want to recursively compute

$$
f:\mu F\to A,
$$

you give an algebra

$$
\alpha:F(A)\to A.
$$

The resulting recursive function is the **fold**

$$
\operatorname{fold}_\alpha:\mu F\to A.
$$

The fundamental diagram is:

$$
\begin{CD}
F(\mu F) @>{F(\operatorname{fold}_\alpha)}>> F(A)\\
@V{\mathsf{in}}VV @VV{\alpha}V\\
\mu F @>>{\operatorname{fold}_\alpha}> A
\end{CD}
$$

or visually:

```text
                 F(f)
     F(μF) -----------------> F(A)
       |                         |
     in|                         |α
       v                         v
      μF ----------------------> A
                 f
```

and the square commutes:

$$
\boxed{
f\circ\mathsf{in}
=
\alpha\circ F(f)
}
$$

This equation is basically the **algebraic form of structural recursion**.

---

# 4. Compare this to your earlier function-space picture

You previously had:

$$
f:X\to Y
$$

and a recursive call

$$
h:R\to X.
$$

The recursion operator was something like

$$
\Phi:Y^X\to Y^X
$$

and recursion means

$$
f=\Phi(f).
$$

The functorial version is:

$$
\boxed{
F(\mu F)\xrightarrow{\mathsf{in}}\mu F
}
$$

and

$$
\boxed{
F(A)\xrightarrow{\alpha}A.
}
$$

Then:

$$
\boxed{
\operatorname{fold}_\alpha:\mu F\to A
}
$$

is the unique function making

$$
\operatorname{fold}_\alpha\circ\mathsf{in}
=
\alpha\circ F(\operatorname{fold}_\alpha)
$$

hold.

So there are really **three levels**:

$$
\boxed{
\begin{array}{ccc}
\text{ordinary function} &:& f:X\to Y\\[2mm]
\text{recursive function} &:& f=\Phi(f)\\[2mm]
\text{structural recursion} &:&
f\circ\mathsf{in}=\alpha\circ F(f)
\end{array}}
$$

The third formulation exposes the **structure being recursively traversed**.

---

## 5. Example: summing a list

For lists,

$$
F(X)=1+A\times X.
$$

The recursive type is

$$
\mu F=\operatorname{List}(A).
$$

Define

$$
\alpha:1+A\times\mathbb N\to\mathbb N
$$

by

$$
\alpha(\ast)=0
$$

and

$$
\alpha(a,n)=a+n.
$$

Then

$$
\operatorname{sum}:\operatorname{List}(A)\to\mathbb N
$$

is the unique map satisfying

$$
\operatorname{sum}\circ\mathsf{in}
=
\alpha\circ F(\operatorname{sum}).
$$

Expanded:

$$
\operatorname{sum}([])
=0
$$

and

$$
\operatorname{sum}(a::xs)
=
a+\operatorname{sum}(xs).
$$

So the familiar recursive program

```text
sum []       = 0
sum (a :: x) = a + sum x
```

is literally the commuting-square equation.

---

### The big picture

You can organize the whole subject as:

$$
\boxed{
\begin{array}{c}
\text{Sets / Types}\\
\downarrow\\
\text{Functions}\\
\downarrow\\
\text{Function spaces }Y^X\\
\downarrow\\
\text{Recursive operators }\Phi:Y^X\to Y^X\\
\downarrow\\
\text{Fixed points }f=\Phi(f)\\
\downarrow\\
\text{Functors }F:\mathcal C\to\mathcal C\\
\downarrow\\
\text{Initial algebras }\mu F\\
\downarrow\\
\text{Folds / catamorphisms}\\
\downarrow\\
\text{General recursion / domain theory}
\end{array}}
$$

The particularly useful distinction is:

$$
\boxed{\text{fixed-point recursion}}
\qquad\text{vs.}\qquad
\boxed{\text{structural recursion}}
$$

The former asks **“what function is a fixed point?”**; the latter asks **“what recursive structure is this function respecting?”**

If you're building the algebraic foundation for programming, I'd study these two diagrams side-by-side rather than treating category theory as a separate subject.
