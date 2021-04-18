---
title: "How to identify if a language is regular or not"
categories:
  - Computer Science
tags:
  - Automata
  - Language
  - Grammar
  - Regularity
last_modified_at: 2021-04-18
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

Learn about regular languages first and identify regularity of the languages.<br/>

## What is regular languages?

A regular language (also called a rational language) is a formal language that can be defined by a regular expression.
Then, what is the regular expression (REGEX)? It uses a given alphabet (Σ), arithmetic expressions (+, •, *), and
primitive regular expressions (Ø, λ). If `r1` and `r2` are regular expressions (REXs), `r1 + r2`, `r1 • r2`, and `r1*` are also regular ones.
- Every regular language `L(r)` has a REGEX `r`.
- Every REGEX `r` has a regular language `L(r)`.

REXs have NFA (or DFA). Conversion proceeds with a generalized transition graph (GTG).
- Every REGEX can build NFA (or DFA).
- NFA (or DFA) can make REXs.

A GTG must be a complete one which all edges are present, so
missing edges should be defined as undefined (Ø).
From every GTG with more than two states, we can also find an arbitary GTG: a GTG with only two states.
The remains are the initial state and a final state.<br/>

In [the previous post](https://tula3and.github.io/computer%20science/automata/),
I mentioned that languages' definition is a little bit ambiguous itself so it needs grammars.
A regular language also has a regular grammar.
Regular grammars must be right or left linear and they can make NFA (or DFA).

In conclusion:
1. A regular language are empty, finite, or infinite.
2. Every empty and finite one is regular. (They can make NFA (or DFA).)
3. REXs ⇔ NFA (or DFA) ⇔ Regular grammars

---

### Example: find a REGEX for language L

L = {w ∈ {0, 1}*: w has no pair of consecutive zeros}

```
r = (1 + 01)*(0 + λ)
```

The image below is an automaton M(r) accepts L((1 + 01)*(0 + λ)).

![nfa](https://user-images.githubusercontent.com/62553200/115142562-6de4cf00-a07d-11eb-9eb6-25795012db25.png)

## Find non-regular languages: pumping lemma

Regular languages are empty, finite, or infinite, but not all infinite ones are regular.
We need a method to figure infinite non-regular languages out: pumping lemma.<br/>

Let L be an infinite regular language.
Then there exists some positive m such that any w ∈ L with ![](https://latex.codecogs.com/gif.latex?%5Cfn_jvn%20%5Csmall%20%7Cw%7C%20%5Cgeq%20m)
can be decomposed as w = xyz with ![](https://latex.codecogs.com/gif.latex?%5Cfn_jvn%20%5Csmall%20%7Cxy%20%7C%5Cleq%20m)
and ![](https://latex.codecogs.com/gif.latex?%5Cfn_jvn%20%5Csmall%20%7Cy%20%7C%5Cgeq%201)
such that ![](https://latex.codecogs.com/gif.latex?%5Cfn_jvn%20%5Csmall%20w_%7Bi%7D%20%3D%20xy%5Eiz) is also in L
for i = 0, 1, 2, ...
While i keeps growing, if there is only one value of i that does not fit to the language definition,
the language is not regular and it is called y cannot be "pumped".
Therefore, we can determine only non-regular languages with pumping lemma, not regular ones.

## References

- [Wikipedia: Regular language](https://en.wikipedia.org/wiki/Regular_language)
- [Wikipedia: Pumping lemma for regular languages](https://en.wikipedia.org/wiki/Pumping_lemma_for_regular_languages)

---

💬 _Any comments and suggestions will be appreciated._
