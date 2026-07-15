The identity used is:

[
\boxed{x \oplus \text{False} = x}
]

where **(\oplus)** denotes **exclusive OR (XOR)**.

### What does XOR mean?

XOR returns **True** if **exactly one** of its operands is True, and **False** if both operands are the same.

Truth table:

| x | y | x ⊕ y |
| - | - | ----- |
| F | F | F     |
| F | T | T     |
| T | F | T     |
| T | T | F     |

---

### Why does (x \oplus \text{False} = x)?

Substitute **False** for the second operand:

| x | False | x ⊕ False |
| - | ----- | --------- |
| F | F     | F         |
| T | F     | T         |

Notice that the result is always exactly the value of (x).

* If (x = \text{False}), then (F \oplus F = F).
* If (x = \text{True}), then (T \oplus F = T).

So XOR with **False** leaves the other operand unchanged.

---

### Intuitive way to remember it

Think of XOR as a **"toggle"** operation:

* XOR with **False (0)**: **do nothing**.
* XOR with **True (1)**: **flip the value**.

For example:

* (0 \oplus 0 = 0)
* (1 \oplus 0 = 1)

No change.

But:

* (0 \oplus 1 = 1)
* (1 \oplus 1 = 0)

The value is inverted.

This is why, in digital circuits and programming, XOR with 1 is often used to toggle a bit.

---

### Other useful XOR identities

| Identity                                        | Meaning                                   |
| ----------------------------------------------- | ----------------------------------------- |
| (x \oplus \text{False} = x)                     | XOR with False does nothing               |
| (x \oplus \text{True} = \neg x)                 | XOR with True flips the value             |
| (x \oplus x = \text{False})                     | A value XOR itself is always False        |
| (x \oplus \neg x = \text{True})                 | A value XOR its complement is always True |
| (x \oplus y = y \oplus x)                       | Commutative                               |
| ((x \oplus y) \oplus z = x \oplus (y \oplus z)) | Associative                               |

These identities are frequently used when simplifying Boolean expressions in logic coverage, digital logic, and computer programming.



Let's derive it step by step and then summarize the most useful XOR expansion rules.

---

# 1. Why is ((a \land c) \oplus a = a \land \neg c)?

Start with the definition of XOR.

[
X \oplus Y=(X\land \neg Y)\ \lor\ (\neg X\land Y)
]

Let

[
X=a\land c,\qquad Y=a
]

Substitute:

[
(a\land c)\oplus a
==================

((a\land c)\land\neg a)
;\lor;
(\neg(a\land c)\land a)
]

### Simplify the first term

[
(a\land c)\land\neg a
=====================

# a\land\neg a\land c

\text{False}
]

because

[
a\land\neg a=\text{False}.
]

So we're left with

[
\neg(a\land c)\land a.
]

### Apply De Morgan's law

[
\neg(a\land c)
==============

\neg a\lor\neg c.
]

Hence,

[
(\neg a\lor\neg c)\land a.
]

Distribute:

# [

(\neg a\land a)
;\lor;
(\neg c\land a).
]

Again,

[
\neg a\land a=\text{False}.
]

Therefore,

# [

a\land\neg c.
]

Hence,

[
\boxed{(a\land c)\oplus a=a\land\neg c.}
]

---

# 2. Faster way

Notice that

[
a=(a\land c);\lor;(a\land\neg c),
]

and these two parts never overlap.

XOR removes the common part:

```
a
= (a∧c) OR (a∧¬c)

(a∧c) XOR a
= (a∧c) XOR [(a∧c) OR (a∧¬c)]
= a∧¬c
```

This trick is often much faster in exams.

---

# 3. XOR expansion formulas

The basic identity is

[
\boxed{X\oplus Y=(X\land\neg Y)\lor(\neg X\land Y)}
]

Everything comes from this.

---

## XOR with AND

### Formula 1

[
(A\land B)\oplus A
==================

A\land\neg B
]

Example:

[
(a\land c)\oplus a
==================

a\land\neg c.
]

---

### Formula 2

[
(A\land B)\oplus B
==================

B\land\neg A.
]

Example:

[
(a\land c)\oplus c
==================

c\land\neg a.
]

---

## XOR with OR

### Formula 3

[
(A\lor B)\oplus A
=================

\neg A\land B.
]

Example:

[
(a\lor c)\oplus a
=================

\neg a\land c.
]

---

### Formula 4

[
(A\lor B)\oplus B
=================

A\land\neg B.
]

Example:

[
(a\lor c)\oplus c
=================

a\land\neg c.
]

---

# 4. Useful XOR identities

| Identity              | Result               |
| --------------------- | -------------------- |
| (A\oplus0)            | (A)                  |
| (A\oplus1)            | (\neg A)             |
| (A\oplus A)           | (0)                  |
| (A\oplus\neg A)       | (1)                  |
| (A\oplus B)           | (B\oplus A)          |
| ((A\oplus B)\oplus C) | (A\oplus(B\oplus C)) |

---

# 5. The four most useful simplifications for logic coverage

These appear frequently in NPTEL Software Testing questions:

| Expression           | Simplifies to   |
| -------------------- | --------------- |
| ((A\land B)\oplus A) | (A\land\neg B)  |
| ((A\land B)\oplus B) | (B\land\neg A)  |
| ((A\lor B)\oplus A)  | (\neg A\land B) |
| ((A\lor B)\oplus B)  | (A\land\neg B)  |

These patterns are especially handy when deriving conditions like (p_a), (p_b), and (p_c) for Active Clause Coverage without expanding the full XOR definition every time.

