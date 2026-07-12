Continuing from the categorical framing.

The key correction:

> `Deck.apply(shuffle)` is not saying "Deck owns the composition law". It is merely a convenient syntax for the **action** of the shuffle monoid on the deck space.

The composition law belongs to the shuffle monoid:

$$
\circ_M:M\times M\rightarrow M
$$

not to `Deck`.

---

# 1. Separate the structures

You have three different categories/structures.

## A. Shuffle monoid

Objects:

$$
*
$$

Morphisms:

$$
Hom(*,*)=M
$$

where:

$$
M=\langle inshuffle,outshuffle\rangle
$$

Composition:

$$
\sigma_2\circ\sigma_1
$$

This is where:

* inshuffle + outshuffle combine
* identity exists
* inverses may exist

The deck has nothing to do with this.

---

## B. Deck space

A deck is a function:

$$
d:P\rightarrow C
$$

The set of all valid decks:

$$
D=\operatorname{Bij}(P,C)
$$

This is an object in the "space of decks".

---

## C. Action

The connection is:

$$
\alpha:M\times D\rightarrow D
$$

meaning:

> A shuffle transforms a deck into another deck.

This is analogous to:

$$
Group\times VectorSpace\rightarrow VectorSpace
$$

For example:

$$
GL_n(R)\times V\rightarrow V
$$

The vector space does not own matrix multiplication. Matrices act on vectors.

Similarly:

$$
M
$$

does not belong to the deck.

---

# 2. Better code design

Instead of:

```python
deck.apply(shuffle)
```

you can make the action explicit.

```python
class ShuffleAction:

    def apply(self, shuffle, deck):
        new_cards = [None] * 52

        for position, card in deck:
            new_position = shuffle(position)
            new_cards[new_position] = card

        return Deck(tuple(new_cards))
```

Usage:

```python
action = ShuffleAction()

new_deck = action.apply(
    OutShuffle(),
    deck
)
```

Now the ownership is clearer:

```
ShuffleMonoid
      |
      |
      v
 ShuffleAction
      |
      |
      v
    Deck
```

---

# 3. Why did OO put it on Deck?

Because OO often groups behavior around the receiver:

```python
deck.shuffle(outshuffle)
```

is ergonomic.

But mathematically:

$$
shuffle:Deck\rightarrow Deck
$$

is not a primitive.

The primitive is:

$$
\sigma:P\rightarrow P
$$

and then:

$$
d'=d\circ\sigma^{-1}
$$

The action is derived.

---

# 4. Adding invariants

Now your invariants are not part of the shuffle.

They are observers:

$$
I:D\rightarrow X
$$

Examples:

## Sign

$$
sgn:S_{52}\rightarrow C_2
$$

## Cycle signature

$$
sig:S_{52}\rightarrow \mathbb{N}^{k}
$$

These observe the permutation.

---

# 5. Represent permutation explicitly

Right now your shuffle is:

```python
position -> position
```

Add a wrapper:

```python
class Permutation:

    def __init__(self, function):
        self.function = function


    def __call__(self, i):
        return self.function(i)


    def compose(self, other):
        return Permutation(
            lambda x:
                self(other(x))
        )
```

Now:

$$
\sigma\circ\tau
$$

is explicit.

---

# 6. Signature invariant

Cycle decomposition:

```python
def cycle_signature(perm: Permutation, n=52):

    visited = set()
    cycles = []

    for start in range(n):

        if start not in visited:

            current = start
            length = 0

            while current not in visited:
                visited.add(current)
                length += 1
                current = perm(current)

            cycles.append(length)

    return tuple(sorted(cycles))
```

Example:

```python
sig = cycle_signature(
    Permutation(OutShuffle())
)

print(sig)
```

Output might be:

```
(1,51)
```

depending on the permutation.

---

# 7. Sign invariant

Mathematically:

$$
sgn(\sigma)=(-1)^{n-c}
$$

where:

* (n=52)
* (c)=number of cycles

```python
def sign(perm: Permutation):

    cycles = cycle_signature(perm)

    return (-1) ** (52 - len(cycles))
```

Now:

```python
sigma = Permutation(OutShuffle())

print(sign(sigma))
```

returns:

$$
+1
$$

or:

$$
-1
$$

---

# 8. Add observers

This is the Observer pattern.

```python
class PermutationObserver:

    def observe(self, permutation):
        raise NotImplementedError



class SignObserver:

    def observe(self, permutation):
        return sign(permutation)



class CycleObserver:

    def observe(self, permutation):
        return cycle_signature(permutation)
```

Usage:

```python
observers = [
    SignObserver(),
    CycleObserver()
]


for observer in observers:
    print(observer.observe(sigma))
```

---

# 9. Full categorical pipeline

Now your architecture is:

$$
\boxed{
G
\rightarrow
G^*
\rightarrow
M
\rightarrow
S_{52}
\rightarrow
D
}
$$

with:

### Generate commands

$$
G^*
$$

### Interpret commands

$$
\rho:G^*\rightarrow M
$$

### Act on decks

$$
\alpha:M\times D\rightarrow D
$$

### Observe

$$
I:D\rightarrow X
$$

For sign:

$$
X=C_2
$$

---

The important design principle:

* **Shuffle owns composition**
  $$
  M\times M\rightarrow M
  $$

* **Action owns applying shuffle**
  $$
  M\times D\rightarrow D
  $$

* **Observers own measurement**
  $$
  D\rightarrow X
  $$

* **Factory owns creation**
  $$
  Raw\rightarrow D
  $$

That separation mirrors the category theory exactly.
