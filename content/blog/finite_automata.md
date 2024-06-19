+++
title = "Understanding The Finite Automata"
date = 2024-06-19
+++

In this article, We'll talk about one of the fundamental concepts in computer science - The Finite Automata. We'll look what exactly it is and understand where can they be used. 

## What is a Finite Automata?

A one word answer would be - a recognizer.
It simply tells whether a string belongs to a language or not.

It is something like asking it, whether the string - **अभिलेख** belongs to the English Language and it simply replies **No**.

I'll use the transition diagram to demonstrate how it exactly works.

### Transition Diagram Basics

1) Every Transition diagram has a starting state and is represented by:

![Start State of a Finite Automata](/img_fa/start-state.png)

2) Every Transition diagram has a final state and is represented by:

![Final State of a Finite Automata](/img_fa/final-state.png)

3) To indicate that the automata will transition to state `B` from `A` when a symbol `a` is encountered we use:

![Transition from state A to B when a symbol a is encountered](/img_fa/transition-from-a-to-b.png)

4) To indicate that the automata will not transition to another state when a symbol a is encountered we use:

![Self Transition in Finite Automata](/img_fa/self-transition.png)

**Note:** We refer the arrow head in transition diagram as an **edge** and that 'a' above the edge as a **label**.

### Types of Finite Automata 

Finite Automata are of **two** types:

#### 1. Non-Deterministic Finite Automata (NFA)

Lets see a transition diagram first. Let me just remind you this is not a complete diagram, it is missing a start and end state.

![Simple NFA](/img_fa/simple-nfa.png)

Things to note here:

a. The state `B` has two edges with the same label `a` to two different states - `C` and `B` itself. This is what non-deterministic means, you cannot tell where the automata will transition to.

b. The state `B` doesn't care about the symbol `b` i.e., it has no edge with the label `b`.

c. The state `C` also doesn't care about the symbol `a`, i.e., it has no edge with label `a`.

d. **NFA** allows **epsilon transition**, i.e. you can set transition from one state to another state on empty string as well.

#### 2. Deterministic Finite Automate (DFA)

Lets see a transition diagram first.

![Simple DFA](/img_fa/simple-dfa.png)

Comparing the above image with the previous image we get the following differences:

a. State `B` has two edges with different labels. We cannot have multiple edges with same labels from a state in **DFA**.

b. Both state `B` and `C` now care about the symbols `b` and `a` respectively. In **DFA** there must a edge for every symbols in the language from every state.

c. **DFA** don't allow **epsilon transition**.

*You might have felt, NFA more powerful, but let me tell you that they have the same power and both of them are capable of recognizing the same langauge- **Regular Language**. It can be proved that they are equivalent but not here, for sure.*

Before we dive more into Finite Automata, lets look at some basic fundamental required:

#### 1.Alphabet

Alphabet is a finite, non-empty set whose elements are symbols.

#### 2. String

A string is a sequence of alphabet. It is denoted by `w` and `|w|` denotes its length.

#### 3. Concatenation

If `x` and `y` are two strings or symbols, then their concatenation xy means, string `x` followed by string `y`.

#### 4. OR

OR is generally denoted by a `+` or `|`. Notation like `x|y` means either `x` or `y`.

#### 5. Kleen Closure

Kleen Closure, denoted by a `*`, means one or more occurrence. Notation like (x)* mean one or more occurrence of `x`.

I think its time to get somewhat formal.

## Finite Automata Again

Finite Automata is a model of computation that recognizes regular language. It has finite set of states, edges labelled with symbols that leads to states and a transition function which tells where to transit, on a particluar input symbol.

Basically, we pass in, a string to a finite automate it transits through multiple states according to the transition function, if it reaches the end of the string and it is at the final (end) state, the automata is said to accept the string. 

If it reaches the end of the string and it is not at the final state, the automata is said to reject the string (just like how she rejected me!!).

I'll now present you a example of designing a Finite Automate - DFA.

#### **A Finite Automata for Language, that has two symbols - a and b where the number of b's is even.**

##### Thought Process

a. We can start by thinking the strings that belongs to this langauge - `a` (Since 0 is even and it has even number of b so it must accepted by our automata.), abb, bb, aabb, abbbb etc and try working out with those strings.

b. We should do something, such that we are able to count the number of b's, while we are reading through the string, we should be at the final state whenever the number of `b` is even.

Lets start by assuming that we'll need just two states - A and B. A being both the start state and B the final state.

![Two States of Finite Auotmata](/img_fa/two_unconnected_states.png)


Remember what I said earlier, we have to be at the final state when the number of `b` is even. So we have to remain at the state `A` when we get a `a` because a single `a` is even `b`.

![Transition to state A when symbol a is encountered while on state A](/img_fa/first-transition.png)

Now we need to think of, what to do when we get a `b` at the state `A` - The start state. Since a single `b` should be rejected, we should move to the state `B`.

![Transition to state B when a symbol b is encountered while on state B](/img_fa/second-transition.png)

We'll be at state `B` after we read a `b` when we are at state `A`. So when we get another `b` it's the second `b` i.e., even number of b's and we have to be at the final state whenever we encounter even b's. So ,

![Transition to state A when a symbol a is encounterd while on state A](/img_fa/third-transition.png)

Ok the final question, what to do when we are at state `B` and we encounter a `a`? - With the configuration so far we'll be at state `B` when we have odd number of `b`s. So no matter how many `a`'s we encounter we need to stay at state `B`. So,

![Transition to state B when a symbol a is encountered while on state B](/img_fa/fourth-transition.png)

This is it, the final configuration for our DFA.

Now, lets see how we can define the transition function for this DFA.

A transition function takes two inputs - the current state and the input. It outputs the state where the automata should transit to.

So, when we are at the state `A` and encounter the symbol `a` we stay at the state `a`, so we write

```
TF(A, a) = A
```
Similarly when we are at state `A` and encounter the symbol `b` we move to the state `B`, so 

```
TF(A, b) = B 
```

Similarly we can write,

```
TF(B, a) = B
TF(B, b) = A
```
## Usage 

1. [Flex](https://en.wikipedia.org/wiki/Flex_(lexical_analyser_generator)) - The Lexical analyzer generator, uses the concept of Finite Automata.



