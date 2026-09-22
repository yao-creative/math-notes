**Intent: comparative mastery / Socratic curriculum.** The goal should be to make you *derive* why each type-theoretic extension exists, rather than memorize a taxonomy.

I would use the same pattern at every level:

$$
\text{phenomenon}
\rightarrow
\text{minimal language}
\rightarrow
\text{typing judgment}
\rightarrow
\text{limitation}
\rightarrow
\text{extension}
$$

Here is a progressively harder sequence.

---

# 0. Simply Typed λ-calculus

### Core idea

Types do **not** depend on terms.

$$
\Gamma\vdash t:A
$$

Example:

$$
\mathsf{compose} :
(B\to C)\to(A\to B)\to A\to C
$$

### Challenge 0.1 — distinguish term/type levels

Suppose

$$
f:A\to B,\qquad x:A.
$$

Can you derive:

$$
\Gamma\vdash f(x):B?
$$

What is the exact role of $\Gamma$?

---

### Challenge 0.2 — derive application

Recover the rule

$$
\frac{\Gamma\vdash f:A\to B\qquad\Gamma\vdash x:A}
{\Gamma\vdash f\,x:B}.
$$

Then ask yourself:

> Why does the system need the domain of $f$ to literally equal the type of $x$?

---

### Challenge 0.3 — find the limitation

Suppose you want a function

$$
\mathsf{head} : \mathsf{Vec}(A,n+1)\to A.
$$

Can STLC express the fact that the input must be **nonempty**?

If not, identify precisely which dependency is missing.

---

# 1. Polymorphic type theory — System F

Now ask:

> What if I don't want one function for `Int`, another for `Bool`, another for `String`, etc.?

Consider:

$$
\mathsf{id}_A:A\to A.
$$

We want one term representing identity for **every** type.

System F introduces:

$$
\forall A.\;A\to A.
$$

and type abstraction:

$$
\Lambda A.\;t.
$$

### Challenge 1.1

What should this expression mean?

$$
(\Lambda A.\lambda x:A.x)[\mathsf{Bool}]
$$

What type does it have?

---

### Challenge 1.2

Explain the difference between:

$$
\lambda x:A.x
$$

and

$$
\Lambda A.\lambda x:A.x.
$$

One abstracts over a **term**.

The other abstracts over a **type**.

---

### Challenge 1.3 — parametricity

Suppose

$$
f:\forall A.\;A\to A.
$$

Without executing $f$, what can it actually do to an arbitrary $x:A$?

Could it:

1. return $x$?
2. manufacture a new arbitrary $A$?
3. inspect whether $A$ is `Bool`?
4. convert $A$ to `Int`?

This is the beginning of **parametricity**.

---

# 2. Dependent type theory

Now introduce the crucial transition:

$$
\boxed{\text{types may depend on terms}}
$$

Instead of

$$
\mathsf{Vec}(A)
$$

we can have

$$
\mathsf{Vec}(A,n).
$$

### Challenge 2.1

Suppose

$$
n:\mathbb N.
$$

Why is

$$
\mathsf{Vec}(A,n)
$$

not just an ordinary type constructor?

Precisely identify the dependency:

$$
n
\longmapsto
\mathsf{Vec}(A,n).
$$

---

### Challenge 2.2

Consider:

$$
\mathsf{head} :
\Pi_{n:\mathbb N}
\mathsf{Vec}(A,n+1)\to A.
$$

Explain why this is stronger than merely:

$$
\mathsf{head}:\mathsf{Vec}(A)\to A.
$$

What invalid inputs have disappeared from the domain?

---

### Challenge 2.3 — dependent function

Compare:

$$
A\to B
$$

with

$$
\Pi_{x:A}B(x).
$$

When does the latter collapse to the former?

---

### Challenge 2.4 — dependent pair

Construct the type:

$$
\Sigma_{n:\mathbb N}\mathsf{Vec}(A,n).
$$

What does an inhabitant look like?

You should arrive at something like:

$$
(n,v)
$$

where

$$
n:\mathbb N
\qquad
v:\mathsf{Vec}(A,n).
$$

Now notice the symmetry:

$$
\Pi \sim \text{dependent function}
$$

$$
\Sigma \sim \text{dependent pair}.
$$

---

# 3. Martin-Löf type theory

Now the interesting question becomes:

> What does **equality itself** look like?

Instead of treating equality as an external relation, introduce:

$$
\mathsf{Id}_A(a,b)
$$

as a type.

### Challenge 3.1

If

$$
p:\mathsf{Id}_A(a,b),
$$

what kind of thing is $p$?

Is it:

* a Boolean?
* a proposition?
* a proof?
* a term?
* a set-theoretic relation?

The correct answer forces you to separate **syntax, semantics, and propositions-as-types**.

---

### Challenge 3.2 — reflexivity

There is a constructor:

$$
\mathsf{refl}_a:\mathsf{Id}_A(a,a).
$$

Why does the theory give you this for free?

Can you construct:

$$
\mathsf{Id}_A(a,b)
$$

when $a$ and $b$ are definitionally different?

---

### Challenge 3.3 — equality elimination

Suppose

$$
p:a=b.
$$

Why should you be able to transport:

$$
P(a)
$$

into

$$
P(b)?
$$

Try to formulate the operation:

$$
\mathsf{transport}_P(p):P(a)\to P(b).
$$

This is one of the most important conceptual steps in dependent type theory.

---

# 4. Calculus of Constructions

Now combine:

* dependent functions,
* dependent types,
* polymorphism,
* higher-order abstraction.

The key conceptual question is:

> What happens if **types themselves can be arguments to functions**, and types can themselves depend on types?

For example:

$$
\Lambda A.\lambda x:A.x.
$$

### Challenge 4.1

What is the type of:

$$
\Lambda A.\lambda x:A.x?
$$

You should derive something equivalent to:

$$
\Pi_{A:\mathcal U}A\to A.
$$

---

### Challenge 4.2

Now ask:

> What is $\mathcal U$?

If types are themselves terms, they need some type.

So you get universes:

$$
A:\mathcal U.
$$

Then ask:

$$
\mathcal U:\;?
$$

Why is the obvious answer

$$
\mathcal U:\mathcal U
$$

dangerous?

This leads directly toward **universe hierarchies**.

---

# 5. Calculus of Inductive Constructions

Now introduce **inductive types**.

Start with:

$$
\mathbb N
$$

generated by:

$$
0:\mathbb N
$$

and

$$
\mathsf{succ}:\mathbb N\to\mathbb N.
$$

### Challenge 5.1

Can you define addition purely through the constructors?

Derive:

$$
\mathsf{add}:\mathbb N\to\mathbb N\to\mathbb N.
$$

---

### Challenge 5.2

Why isn't an inductive type merely a set containing some elements?

For example, why does specifying

$$
0,\mathsf{succ}
$$

also give you an **elimination/recursion principle**?

This is the beginning of understanding inductive types as **initial algebra-like objects**.

---

### Challenge 5.3 — dependent induction

Define:

$$
\mathsf{Vec}(A,n).
$$

Then prove:

$$
\mathsf{reverse}:
\mathsf{Vec}(A,n)\to\mathsf{Vec}(A,n).
$$

The challenge isn't writing code.

The challenge is:

> How does the type of the recursive result change as the index changes?

---

# 6. Intensional vs extensional type theory

This is an extremely important fork.

Suppose you have:

$$
t,u:A.
$$

There are two notions to distinguish.

### Definitional equality

For example:

$$
(\lambda x.x)\;a
\equiv
a.
$$

The type checker can **compute** this.

### Propositional equality

You may instead have:

$$
p:t=u.
$$

where the equality requires a proof.

---

### Challenge 6.1

Is:

$$
t\equiv u
$$

the same thing as:

$$
t=u?
$$

If not, construct a conceptual example where two terms are propositionally equal but not definitionally equal.

---

### Challenge 6.2

Why would a programming-language designer care enormously about keeping definitional equality decidable?

This takes you directly into the boundary between:

$$
\text{proof theory}
\quad\text{and}\quad
\text{computation}.
$$

---

# 7. Homotopy Type Theory

Now take identity types seriously.

Suppose:

$$
p:a=b.
$$

Then $p$ is itself a term.

But perhaps:

$$
p,q:a=b.
$$

Can we have:

$$
r:p=q?
$$

Yes.

Then potentially:

$$
s:r=s?
$$

and so on.

You get:

$$
a,b
$$

connected by

$$
p:a=b
$$

with paths between paths, etc.

### Challenge 7.1

Why does ordinary set theory tend to collapse this structure?

For a set $A$, equality behaves propositionally:

$$
a=b
$$

has essentially no interesting higher-dimensional structure.

But in type theory, identity types can have many inhabitants.

---

### Challenge 7.2

Consider:

$$
p:a=b
$$

and a function:

$$
f:A\to B.
$$

Can you construct:

$$
\mathsf{ap}_f(p):f(a)=f(b)?
$$

Why should every function preserve equality?

---

# 8. Univalence

Now consider two types:

$$
A\simeq B.
$$

They might be structurally equivalent without being literally the same syntactic type.

Univalence asks:

> Should an equivalence itself induce an identity between types?

Symbolically:

$$
(A\simeq B)\simeq(A=B).
$$

### Challenge 8.1

Take:

$$
\mathbb N\times 1.
$$

It is equivalent to:

$$
\mathbb N.
$$

Should mathematics regard these as “the same”?

Separate these three notions:

$$
\text{syntactic equality}
$$

$$
\text{definitional equality}
$$

$$
\text{mathematical equivalence}.
$$

Then ask why univalence wants the third to participate in the second-level structure of equality.

---

# 9. Cubical Type Theory

HoTT gives you sophisticated equality, but you then face:

> Can equality be made computational?

Cubical type theory introduces an interval:

$$
\mathbb I
$$

with endpoints:

$$
0,1:\mathbb I.
$$

A path can then be represented computationally as something like:

$$
p:\mathbb I\to A
$$

with

$$
p(0)=a,\qquad p(1)=b.
$$

### Challenge 9.1

Why does this look much more like an ordinary function than an abstract equality proof?

---

### Challenge 9.2

If:

$$
p:\mathbb I\to A
$$

is a path from $a$ to $b$, what operation should reverse the path?

Try:

$$
p^{-1}(i)=p(1-i).
$$

Then derive:

$$
p^{-1}:b=a.
$$

This is where the geometric interpretation becomes computational.

---

# 10. Linear type theory

Now take a completely different axis.

Ordinary typing allows:

$$
x:A\vdash(x,x):A\times A.
$$

You duplicated $x$.

Linear type theory says:

> If you have one resource, you must use it exactly once.

### Challenge 10.1

Why is this useful for something like:

* file handles,
* sockets,
* memory,
* transactions,
* capabilities?

---

### Challenge 10.2

Suppose:

$$
f:A\multimap B.
$$

and

$$
x:A.
$$

After evaluating:

$$
f(x),
$$

can you still use $x$?

Why or why not?

---

# 11. Substructural type theory

Linear logic reveals that ordinary logic secretly assumes structural rules:

$$
\frac{\Gamma\vdash A}{\Gamma,x:A\vdash A}
$$

(**weakening**)

and

$$
\Gamma,x:A,x:A\vdash B
\quad\Rightarrow\quad
\Gamma,x:A\vdash B
$$

(**contraction**).

### Challenge 11.1

What programming operation corresponds to contraction?

Answer:

$$
\boxed{\text{copying}}
$$

What corresponds to weakening?

$$
\boxed{\text{discarding}}
$$

Now ask:

> What happens to programming if the type system controls both operations?

This gets you directly toward Rust's ownership/borrowing discipline.

---

# 12. Modal type theory

Now ask whether a value is available:

* now,
* later,
* in another stage,
* with another capability,
* under some modality.

A modal type might look like:

$$
\Box A.
$$

### Challenge 12.1

Why shouldn't an ordinary:

$$
A
$$

automatically be usable where:

$$
\Box A
$$

is expected?

What additional information does $\Box$ encode?

---

# 13. Session types

This is particularly useful for systems programming.

Instead of typing just a value:

$$
x:\mathsf{Socket}
$$

you type the **protocol state** of the socket.

For example:

$$
\mathsf{Send}(A,\mathsf{Recv}(B,\mathsf{End})).
$$

Meaning roughly:

$$
\text{send }A
\rightarrow
\text{receive }B
\rightarrow
\text{terminate}.
$$

### Challenge 13.1

After performing the send operation, should the socket still have its original type?

No.

Its type should transition:

$$
\mathsf{Send}(A,S)
\longrightarrow
S.
$$

This is essentially **state encoded in the type**.

---

# The deeper challenge: unify the whole thing

After working through these, I would give you these questions **without telling you the answers**.

### Q1

What is the fundamental difference between:

$$
A\to B
$$

and

$$
\Pi_{x:A}B(x)?
$$

---

### Q2

What is the fundamental difference between:

$$
A\times B
$$

and

$$
\Sigma_{x:A}B(x)?
$$

---

### Q3

Why are:

$$
\Pi
$$

and

$$
\Sigma
$$

not arbitrary notation, but the type-theoretic analogues of universal/existential structure?

---

### Q4

Why does:

$$
\mathsf{Id}_A(a,b)
$$

have to be a **type** rather than merely a Boolean relation if we want equality to interact naturally with dependent types?

---

### Q5

Why does allowing:

$$
\mathcal U:\mathcal U
$$

create a foundational problem?

---

### Q6

What is the precise conceptual transition:

$$
\text{STLC}
\rightarrow
\text{System F}
\rightarrow
\text{Dependent TT}
$$

?

You should be able to phrase each transition as **what new thing can depend on what**.

---

### Q7

Then the really important one:

$$
\boxed{
\text{What information is represented by a type that cannot be represented by an ordinary set?}
}
$$

And conversely:

$$
\boxed{
\text{What information does ordinary set theory represent naturally that intensional type theory does not?}
}
$$

If you can answer those two precisely, you have moved beyond “type theory = Curry–Howard” into actually understanding the foundational design space.
