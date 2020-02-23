+++
title = "Object-Oriented Programming with Go"

date = 2020-02-23T20:00:00+08:00
author = "Mingding Han"
+++

# Concepts Recap
Key Principles: Encapsulation & Composition

## Methods
- Value Receivers
- Pointer Receivers
- Method Values & Expressions
- Composing Types by Struct Embedding

---

# Exercises
### Ex1: Point & Path

https://play.golang.org/p/X6CCXNes780

### Ex2: IntSet (Bit Vector)

- Sets in Go are usually implemented as a `map[T] bool`, where `T` is the element type
- However, a bit vector could be more suitable for the following situations:
  - set elements are small non-negative integers
  - sets have many elements
  - set operation like union and intersection are common
- A bit vector uses a slice of unsigned integer values, or *words*
  - Each bit in a word represents a possible element of the set
  - The set contains *i* if the *i*-th bit is set

w.i.p https://play.golang.org/p/Z8MYIv-FLsf

---

# References
- # gopl Chapter 6
