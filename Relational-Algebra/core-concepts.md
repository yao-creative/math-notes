Relational algebra is the **algebra of manipulating relations as sets**. The core idea is:

> A database relation is a mathematical set of tuples, and relational algebra defines closed operations that transform relations into new relations.

The important word is **closed**:

$$
f: \text{Relation}^n \rightarrow \text{Relation}
$$

Every operation consumes relations and produces another relation.

---

# 1. Primitive object: Relation

A relation is a subset of a Cartesian product.

Given domains:

$$
D_1, D_2, \dots, D_n
$$

an $n$-ary relation:

$$
R \subseteq D_1 \times D_2 \times \dots \times D_n
$$

Example:

Student table:

$$
Student \subseteq Name \times Age \times Major
$$

A tuple:

$$
(\text{"Alice"}, 20, \text{"CS"})
$$

is an element:

$$
(\text{"Alice"}, 20, \text{"CS"}) \in Student
$$

---

# 2. Schema vs instance

Separate two concepts.

## Schema

The structure:

$$
Student(Name, Age, Major)
$$

Mathematically:

$$
S = (A_1, A_2, A_3)
$$

where attributes are names/types.

---

## Instance

The actual set:

$$
I(Student) = \{\, (a, 20, CS),\ (b, 21, Math) \,\}
$$

So:

$$
\text{Relation} = (\text{Schema}, \text{Set of tuples})
$$

---

# 3. Relational algebra operators

The algebra is a collection of functions.

---

# Selection (filter rows)

Notation:

$$
\sigma_\phi(R)
$$

Meaning:

Keep tuples satisfying predicate $\phi$.

Formal:

$$
\sigma_\phi(R) = \{\, t \in R \mid \phi(t) = true \,\}
$$

Example:

$$
\sigma_{Age > 20}(Student)
$$

is:

$$
\{\, t \in Student \mid t.Age > 20 \,\}
$$

Set-theoretically:

$$
\sigma_\phi: \mathcal{P}(A) \rightarrow \mathcal{P}(A)
$$

---

# Projection (remove attributes)

Notation:

$$
\pi_X(R)
$$

where $X$ is a subset of attributes.

Formal:

$$
\pi_X(R) = \{\, t[X] \mid t \in R \,\}
$$

Example:

$$
\pi_{Name}(Student)
$$

turns:

$$
(Name, Age, Major)
$$

into:

$$
(Name)
$$

---

# Union

Requires compatible schemas.

$$
R \cup S
$$

Formal:

$$
R \cup S = \{\, x \mid x \in R \lor x \in S \,\}
$$

Type:

$$
\cup: Rel(A) \times Rel(A) \rightarrow Rel(A)
$$

---

# Difference

$$
R - S
$$

Formal:

$$
R - S = \{\, x \in R \mid x \notin S \,\}
$$

---

# Cartesian product

Combine every tuple.

$$
R \times S
$$

Formal:

$$
R \times S = \{\, (r,s) \mid r \in R \land s \in S \,\}
$$

Type:

$$
Rel(A) \times Rel(B) \rightarrow Rel(A \times B)
$$

---

# Join (most important)

Join is constrained product.

Natural join:

$$
R \Join S
$$

Conceptually:

$$
R \Join S = \sigma_{condition}(R \times S)
$$

So:

$$
\boxed{Join = Product + Selection}
$$

Example:

Employee:

$$
Employee(EID, Name, DeptID)
$$

Department:

$$
Department(DeptID, DeptName)
$$

Join:

$$
Employee \Join Department
$$

matches:

$$
Employee.DeptID = Department.DeptID
$$

---

# 4. Relational algebra as a query language

A SQL query:

```sql
SELECT Name
FROM Student
WHERE Age > 20;
```

is:

$$
\pi_{Name}\big(\sigma_{Age > 20}(Student)\big)
$$

The execution order:

$$
Student \rightarrow \sigma \rightarrow \pi
$$

---

# 5. Algebraic laws

Because relations are sets, they inherit set algebra.

## Selection composition

$$
\sigma_a(\sigma_b(R)) = \sigma_{a \land b}(R)
$$

Filtering twice equals one combined filter.

---

## Projection idempotence

$$
\pi_A(\pi_A(R)) = \pi_A(R)
$$

Applying projection twice does nothing.

---

## Join associativity

$$
(R \Join S) \Join T = R \Join (S \Join T)
$$

This allows query planners to reorder joins.

---

# 6. Relational algebra as functions

The whole system:

Let:

$$
\mathcal{R}_A = \mathcal{P}(A)
$$

be all possible relations over schema $A$.

Then:

Selection:

$$
\sigma_\phi: \mathcal{R}_A \rightarrow \mathcal{R}_A
$$

Projection:

$$
\pi_X: \mathcal{R}_{A \times B} \rightarrow \mathcal{R}_A
$$

Join:

$$
\Join: \mathcal{R}_A \times \mathcal{R}_B \rightarrow \mathcal{R}_{A \cup B}
$$

So relational algebra is literally a typed algebra over sets.

---

# 7. Category theory view

A useful abstraction:

Objects:

$$
\text{Relations}
$$

Morphisms:

$$
\text{Relational transformations}
$$

Operations:

$$
F: \mathbf{Rel}^n \rightarrow \mathbf{Rel}
$$

The category of relations:

$$
\mathbf{Rel}
$$

has:

* objects = sets
* morphisms = relations

A relation itself:

$$
R: A \rightarrow B
$$

is not a function:

$$
a \mapsto b
$$

but a subset:

$$
R \subseteq A \times B
$$

Composition:

$$
S \circ R = \{\, (a,c) \mid \exists\, b: (a,b) \in R \land (b,c) \in S \,\}
$$

This is why relational algebra connects deeply to logic, databases, and category theory.

---

## Compression

The whole subject reduces to:

$$
\boxed{\text{Relation} = \text{set of tuples}}
$$

$$
\boxed{\text{Relational algebra} = \text{closed operations over tuple sets}}
$$

Core operators:

$$
\boxed{\sigma, \pi, \cup, -, \times, \Join}
$$

with:

$$
\boxed{Join = Product + Constraint}
$$

and SQL is essentially a higher-level syntax for these transformations.