```python
from enum import Enum
from pydantic import BaseModel,model_validator
from dataclasses import dataclass
from abc import ABC, abstractmethod
from typing import Callable


#Immutable
# forces pure transformation of the data
# nums already immutable
class Suit(str, Enum):
    HEARTS = "♥"
    SPADES = "♠"
    CLUBS = "♣"
    DIAMONDS = "♦"

#Immutable so that the state \times axis won't be an issue when we check rank = Rank() some other time
class Rank(str, Enum):
    A = "A"
    K = "K"
    Q = "Q"
    J = "J"
    TEN = "10"
    NINE = "9"
    EIGHT = "8"
    SEVEN = "7"
    SIX = "6"
    FIVE = "5"
    FOUR = "4"
    THREE = "3"
    TWO = "2"

#Immutable pydantic offers this
class Card(BaseModel, frozen=True):
    suit: Suit
    rank: Rank


STANDARD_DECK = tuple(
    Card(rank=r, suit=s)
    for s in Suit
    for r in Rank
)

POSITIONS = tuple(range(len(STANDARD_DECK)))

#Immutable


class DeckInterface(ABC):

    @abstractmethod
    def card_at(self, position: int) -> Card:
        pass
    
    @abstractmethod
    def position_of(self, card: Card) -> int:
        pass

    # change the permutation of the 
    @abstractmethod
    def shuffle(self, permutation):
        pass
    

class Deck(BaseModel):
    #implicit total order lattice on tuple.
    cards: tuple[Card, ...]

    model_config = {
        "frozen": True
    }

    @classmethod
    def standard(cls):
        return cls(cards=STANDARD_DECK)


    # d: P -> C, deck morphism taking position to card
    def card_at(self, position: int):
        return self.cards[position]

    # d^{-1}
    def position_of(self, card: Card):
        return self.cards.index(card)

    def compute_invariant(self):
        return 
    def __iter__(self):
        return enumerate(self.cards)

    @model_validator(mode="after")
    def validate(self):
        if len(self.cards) != 52:
            raise ValueError()

        if len(set(self.cards)) != 52:
            raise ValueError()

        return self


class Shuffle(ABC):
    @abstractmethod
    def __call__(self, *args, **kwargs):
        pass

# monoid category which contains two morphisms, out shuffle and inshuffle which are pure endomorphism on P -> P
class OutShuffle(Shuffle):
    def __call__(self, i: int) -> int:
        if i < 26:
            return 2*i
        else:
            return 2*(i-26)+1

class InShuffle(Shuffle):
    def __call__(i:int)->int:
        if i < 26:
            return 2*i+1
        else:
            return 2*(i-26)

class IdShuffle(Shuffle):
    def __call__(i:int) -> int:
        return lambda x:x

MONOID_MORPHISMS = {InShuffle, OutShuffle, IdShuffle}

# The monoid composition law. #equivalent to playing/ fold
class CompositeShuffle:
    def __init__(self, *operations):
        self.operations = operations

    def __call__(self, position):
        result = position

        for op in reversed(self.operations):
            result = op(result)

        return result


# Do not let callers create invalid decks.
class DeckFactory:
    @staticmethod
    def standard():
        return Deck(tuple(STANDARD_DECK))

    @staticmethod
    def from_cards(cards):
        validate(cards)
        return Deck(tuple(cards))



generators = {
    "in": InShuffle,
    "out": OutShuffle
}

# Invariant Morphism which I found to be sign, aka the induction predicate which always holds is that inshuffle out outshuffle will never reach Alternating group A_n
def signature(perm):
    visited=set()
    cycles=[]

    for i in range(52):
        if i not in visited:
            cycle=[]
            j=i

            while j not in visited:
                visited.add(j)
                cycle.append(j)
                j=perm(j)

            cycles.append(len(cycle))

    return tuple(sorted(cycles))

def sign(perm):
    cycles = signature(perm)
    return (-1) ** (52 - cycles)
```

I assume you mean **Factory Pattern** (not "factor"). The core problem it solves is:

> **How do we create objects without making the caller know the concrete construction details?**

In your deck example, this is exactly the boundary between:

$$
\text{arbitrary data}
\rightarrow
\text{valid mathematical object}
$$

---

# 1. The problem: construction logic leaks

Without a factory:

```python
deck = Deck(
    cards=[
        Card(Suit.HEARTS, Rank.A),
        ...
    ]
)
```

Every caller now needs to know:

* what fields exist
* what representation is used
* how to validate
* what defaults exist
* how to create special cases

Example:

```python
game.py
```

```python
deck = Deck(cards=STANDARD_DECK)
```

```python
test.py
```

```python
deck = Deck(cards=tuple(cards))
```

```python
simulation.py
```

```python
deck = Deck(cards=shuffle(cards))
```

Now construction rules are scattered.

The invariant:

$$
Deck \in \operatorname{Bij}(P,C)
$$

is not centralized.

---

# 2. Factory centralizes creation

Instead:

```python
class DeckFactory:

    @staticmethod
    def standard():
        return Deck(cards=STANDARD_DECK)

    @staticmethod
    def shuffled(shuffle):
        cards = apply_shuffle(STANDARD_DECK, shuffle)
        return Deck(cards=cards)
```

Usage:

```python
deck = DeckFactory.standard()
```

The caller says:

> I need a standard deck.

not:

> I know how a deck is represented internally.

---

# 3. Factory solves representation independence

Suppose today:

```python
class Deck:
    cards: tuple[Card,...]
```

Tomorrow you optimize:

```python
class Deck:
    permutation: array[int]
```

Without factory:

every caller breaks.

With factory:

```python
deck = DeckFactory.standard()
```

still works.

The morphism:

$$
Raw \rightarrow Deck
$$

has not changed.

Only the implementation changed.

---

# 4. Factory solves invalid intermediate states

This is the most important for your mathematical objects.

Suppose:

```python
Deck(cards)
```

allows:

```python
[
Ace♠,
Ace♠,
Ace♠
]
```

Now:

$$
Deck \not\subseteq Bij(P,C)
$$

You have a fake deck.

Factory can enforce:

$$
\forall x,\quad Factory(x)\in Deck
$$

Example:

```python
class DeckFactory:

    @staticmethod
    def from_cards(cards):

        if len(cards) != 52:
            raise ValueError

        if len(set(cards)) != 52:
            raise ValueError

        return Deck(cards=tuple(cards))
```

---

# 5. Factory is different from validation

Important distinction:

## Validation

Question:

> Is this object valid?

$$
x\in D?
$$

Example:

```python
validate(cards)
```

---

## Factory

Question:

> How do I produce an object in (D)?

$$
X\rightarrow D
$$

Example:

```python
DeckFactory.standard()
```

---

They are related but different.

A factory may use validation, but validation is not the factory.

---

# 6. Category theory view

Suppose:

$$
R
$$

is raw representation space:

$$
R=Card^{52}
$$

but only some values are valid:

$$
D\subseteq R
$$

The factory is a morphism:

$$
F:R\rightarrow D
$$

where:

$$
F(r)=d
$$

such that:

$$
d\in D
$$

The factory is a **lifting into a constrained subobject**.

Diagram:

$$
\begin{array}{ccc}
& D & \\
& \uparrow F & \\
R & &
\end{array}
$$

---

# 7. Common factory callers

Factories are usually called by:

## Application layer

Example:

```python
game = Game(
    deck=DeckFactory.standard()
)
```

The game does not construct cards.

---

## Tests

```python
deck = DeckFactory.standard()
```

instead of manually creating 52 cards.

---

## Parsers

Example:

```python
deck = DeckFactory.from_json(data)
```

The parser creates valid domain objects.

---

## Dependency injection

Example:

```python
service = Service(
    repository=RepositoryFactory.create()
)
```

---

# 8. Your project architecture

I would separate:

```
Card
 |
 v
DeckFactory
 |
 v
Deck (immutable value)
 |
 v
Shuffle monoid action
 |
 v
Simulator
```

Meaning:

* `Card` = value object
* `DeckFactory` = creation boundary
* `Deck` = mathematical object
* `Shuffle` = algebra
* `Simulator` = orchestration

---

The deeper principle:

**Factories protect the entrance into a domain.**

They answer:

> "How do things become valid members of this mathematical structure?"

For your deck:

$$
\boxed{
\text{Factory creates elements of } \operatorname{Bij}(P,C)
}
$$

The shuffle monoid then acts on those elements:

$$
M\times Deck\rightarrow Deck
$$

which keeps construction and transformation cleanly separated.
