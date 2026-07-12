Good catch. There are two separate things that were conflated:

1. **A shuffle permutation as a function on one position**
   $$
   \sigma:P\rightarrow P
   $$

2. **Applying that permutation to the entire deck**
   $$
   Deck \rightarrow Deck
   $$

The first is algebra. The second is the action.

---

# 1. Why does `Shuffle.__call__` only map one index?

Because mathematically a permutation is:

$$
\sigma\in S_{52}
$$

and:

$$
S_{52}=\operatorname{Bij}(P,P)
$$

So the primitive object is:

$$
\sigma(i)=j
$$

Example:

```python
outshuffle(0) = 0
outshuffle(1) = 2
outshuffle(26)=1
```

This is just the morphism:

$$
P\xrightarrow{\sigma}P
$$

---

# 2. Where is it combined into a whole deck?

The combination happens in the **action**.

A deck is:

$$
d:P\rightarrow C
$$

A shuffle acts on it:

$$
\sigma:P\rightarrow P
$$

The new deck is:

$$
d'=d\circ\sigma^{-1}
$$

So the implementation:

```python
def apply(self, shuffle):
    new_cards = [None]*52

    for old_position, card in enumerate(self.cards):
        new_position = shuffle(old_position)
        new_cards[new_position] = card

    return Deck(tuple(new_cards))
```

is the concrete realization of:

$$
\alpha:S_{52}\times Deck\rightarrow Deck
$$

where:

$$
\alpha(\sigma,d)=d\circ\sigma^{-1}
$$

---

Example:

Before:

```
position: 0 1 2 3 4 ...
card:     A B C D E ...
```

Outshuffle:

$$
0\mapsto 0,\qquad 1\mapsto 2,\qquad 2\mapsto 4
$$

After:

```
position: 0 1 2 3 4 ...
card:     A ? B ? C ...
```

The permutation function only knows:

$$
position \mapsto position
$$

The deck knows:

$$
position \mapsto card
$$

The action combines them.

---

# 3. Now the Command Pattern problem

The problem command solves:

> How do you turn an operation from something that happens immediately into something you can store, pass around, replay, inspect, undo, queue, or serialize?

Without Command:

```python
deck = deck.apply(InShuffle())
deck = deck.apply(OutShuffle())
```

The actions disappear after execution.

You have:

$$
action
\rightarrow
effect
$$

---

With Command:

```python
commands = [
    ShuffleCommand(InShuffle()),
    ShuffleCommand(OutShuffle())
]
```

Now:

$$
action
\rightarrow
object
$$

The action becomes data.

---

# 4. Command implementation

```python
class Command(Protocol):

    def execute(self, receiver):
        ...


class ShuffleCommand:

    def __init__(self, shuffle):
        self.shuffle = shuffle


    def execute(self, deck):
        return deck.apply(self.shuffle)
```

Now:

```python
command = ShuffleCommand(InShuffle())

deck2 = command.execute(deck)
```

The command stores:

$$
\sigma:P\rightarrow P
$$

and later applies:

$$
Deck\xrightarrow{\sigma}Deck
$$

---

# 5. Who calls Command?

The usual pattern:

```
Client
  |
  creates
  v
Command
  |
  stored/executed by
  v
Invoker
  |
  calls
  v
Receiver
```

For your system:

| Pattern role | Your object    |
| ------------ | -------------- |
| Client       | Simulator      |
| Command      | ShuffleCommand |
| Invoker      | Simulator      |
| Receiver     | Deck           |

Example:

```python
class Simulator:

    def __init__(self):
        self.history=[]


    def run(self, command, deck):

        self.history.append(command)

        return command.execute(deck)
```

Usage:

```python
sim = Simulator()

deck = sim.run(
    ShuffleCommand(InShuffle()),
    deck
)
```

---

# 6. Category theory formalization of Command

Normally:

$$
f:X\rightarrow Y
$$

is a morphism.

For your shuffle:

$$
\sigma:P\rightarrow P
$$

or the induced deck transformation:

$$
T_\sigma:Deck\rightarrow Deck
$$

A command packages the morphism:

$$
\sigma
$$

as an object:

$$
Command(\sigma)
$$

with an evaluation map:

$$
eval:Command(X,Y)\times X\rightarrow Y
$$

such that:

$$
eval(Command(f),x)=f(x)
$$

This is the same idea as the **exponential object** in a Cartesian closed category:

$$
Y^X
$$

The function space contains all morphisms:

$$
Hom(X,Y)
$$

A command is essentially:

$$
\text{a morphism + metadata}
$$

---

# 7. Composition of Commands

This is where your monoid appears.

Commands:

$$
c_1,c_2,c_3
$$

compose:

$$
c_3\circ c_2\circ c_1
$$

Your history:

```python
[
 InShuffle,
 OutShuffle,
 InShuffle
]
```

is a word:

$$
w\in G^*
$$

The executor performs the fold:

$$
fold(\circ,w)
$$

giving:

$$
\sigma
$$

---

# 8. Why not just store functions?

You could do:

```python
history=[
    InShuffle(),
    OutShuffle()
]
```

Why Command?

Because Command gives you extra structure:

```python
class ShuffleCommand:

    name="outshuffle"

    timestamp=...

    user="player1"

    def undo(self):
        ...
```

The function only gives:

$$
P\rightarrow P
$$

The command gives:

$$
\text{operation}+\text{context}
$$

---

# 9. The full categorical picture

You have:

### Generator

$$
G={in,out}
$$

↓

### Free monoid

$$
G^*
$$

(all possible command sequences)

↓

### Interpretation

$$
\rho:G^*\rightarrow S_{52}
$$

(actual permutations)

↓

### Action

$$
S_{52}\times Deck\rightarrow Deck
$$

↓

### Observation

$$
Deck\rightarrow C_2
$$

(sign)

So Command is the **syntactic layer**:

$$
\boxed{
\text{Commands} = \text{programs}
}
$$

and Shuffle is the **semantic layer**:

$$
\boxed{
\text{Permutation}=\text{meaning of program}
}
$$

This is exactly the same separation as:

* source code vs compiled program
* expression tree vs evaluated value
* free monoid vs represented monoid

which is why Command fits your shuffle algebra very naturally.
