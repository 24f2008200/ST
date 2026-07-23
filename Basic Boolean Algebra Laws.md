Here's the corrected version with proper LaTeX formatting throughout:

## The XOR Identity

$$x \oplus \text{False} = x$$

where $\oplus$ denotes **exclusive OR (XOR)**.

### What does XOR mean?

XOR returns **True** if exactly one of its operands is True, and **False** if both operands are the same.

Truth table:

| x | y | $x \oplus y$ |
|---|---|---|
| F | F | F |
| F | T | T |
| T | F | T |
| T | T | F |

### Why does $x \oplus \text{False} = x$?

Substitute **False** for the second operand:

| x | False | $x \oplus \text{False}$ |
|---|---|---|
| F | F | F |
| T | F | T |

The result is always exactly the value of $x$.

- If $x = \text{False}$, then $F \oplus F = F$.
- If $x = \text{True}$, then $T \oplus F = T$.

So XOR with **False** leaves the other operand unchanged.

### Intuitive way to remember it

Think of XOR as a **"toggle"** operation:

- XOR with **False (0)**: do nothing.
- XOR with **True (1)**: flip the value.

For example:
$$0 \oplus 0 = 0, \qquad 1 \oplus 0 = 1$$
No change. But:
$$0 \oplus 1 = 1, \qquad 1 \oplus 1 = 0$$
The value is inverted. This is why, in digital circuits and programming, XOR with 1 is often used to toggle a bit.

### Other useful XOR identities

| Identity | Meaning |
|---|---|
| $x \oplus \text{False} = x$ | XOR with False does nothing |
| $x \oplus \text{True} = \neg x$ | XOR with True flips the value |
| $x \oplus x = \text{False}$ | A value XOR itself is always False |
| $x \oplus \neg x = \text{True}$ | A value XOR its complement is always True |
| $x \oplus y = y \oplus x$ | Commutative |
| $(x \oplus y) \oplus z = x \oplus (y \oplus z)$ | Associative |

These identities are frequently used when simplifying Boolean expressions in logic coverage, digital logic, and computer programming.

---

# Derivation: Why is $(a \land c) \oplus a = a \land \neg c$?

Start with the definition of XOR:

$$X \oplus Y = (X \land \neg Y) \lor (\neg X \land Y)$$

Let $X = a \land c$ and $Y = a$. Substitute:

$$(a \land c) \oplus a = \big((a \land c) \land \neg a\big) \;\lor\; \big(\neg(a \land c) \land a\big)$$

**Simplify the first term:**

$$(a \land c) \land \neg a = a \land \neg a \land c = \text{False}$$

because $a \land \neg a = \text{False}$.

So we're left with:

$$\neg(a \land c) \land a$$

**Apply De Morgan's law:**

$$\neg(a \land c) = \neg a \lor \neg c$$

Hence:

$$(\neg a \lor \neg c) \land a$$

**Distribute:**

$$(\neg a \land a) \;\lor\; (\neg c \land a)$$

Again, $\neg a \land a = \text{False}$. Therefore:

$$a \land \neg c$$

Hence:

$$\boxed{(a \land c) \oplus a = a \land \neg c}$$

---

## Faster way

Notice that:

$$a = (a \land c) \lor (a \land \neg c)$$

and these two parts never overlap. XOR removes the common part:

$$
\begin{aligned}
a &= (a \land c) \lor (a \land \neg c) \\
(a \land c) \oplus a &= (a \land c) \oplus \big[(a \land c) \lor (a \land \neg c)\big] \\
&= a \land \neg c
\end{aligned}
$$

This trick is often much faster in exams.

---

## XOR expansion formulas

The basic identity is:

$$\boxed{X \oplus Y = (X \land \neg Y) \lor (\neg X \land Y)}$$

Everything comes from this.

### XOR with AND

**Formula 1:**
$$(A \land B) \oplus A = A \land \neg B$$
Example: $(a \land c) \oplus a = a \land \neg c$

**Formula 2:**
$$(A \land B) \oplus B = B \land \neg A$$
Example: $(a \land c) \oplus c = c \land \neg a$

### XOR with OR

**Formula 3:**
$$(A \lor B) \oplus A = \neg A \land B$$
Example: $(a \lor c) \oplus a = \neg a \land c$

**Formula 4:**
$$(A \lor B) \oplus B = A \land \neg B$$
Example: $(a \lor c) \oplus c = a \land \neg c$

---

## Useful XOR identities

| Identity | Result |
|---|---|
| $A \oplus 0$ | $A$ |
| $A \oplus 1$ | $\neg A$ |
| $A \oplus A$ | $0$ |
| $A \oplus \neg A$ | $1$ |
| $A \oplus B$ | $B \oplus A$ |
| $(A \oplus B) \oplus C$ | $A \oplus (B \oplus C)$ |

---

## The four most useful simplifications for logic coverage

These appear frequently in NPTEL Software Testing questions:

| Expression | Simplifies to |
|---|---|
| $(A \land B) \oplus A$ | $A \land \neg B$ |
| $(A \land B) \oplus B$ | $B \land \neg A$ |
| $(A \lor B) \oplus A$ | $\neg A \land B$ |
| $(A \lor B) \oplus B$ | $A \land \neg B$ |

These patterns are especially handy when deriving conditions like $p_a$, $p_b$, and $p_c$ for Active Clause Coverage without expanding the full XOR definition every time.



Here's a comprehensive revision sheet covering XOR identities, De Morgan's laws, and general Boolean algebra — organized so you can use it as a quick-reference before your exam.

## 1. Basic Boolean Algebra Laws

**Identity laws**
$$A \land 1 = A \qquad A \lor 0 = A$$

**Null (Domination) laws**
$$A \land 0 = 0 \qquad A \lor 1 = 1$$

**Idempotent laws**
$$A \land A = A \qquad A \lor A = A$$

**Complement laws**
$$A \land \lnot A = 0 \qquad A \lor \lnot A = 1$$

**Double negation**
$$\lnot(\lnot A) = A$$

**Commutative laws**
$$A \land B = B \land A \qquad A \lor B = B \lor A$$

**Associative laws**
$$(A \land B) \land C = A \land (B \land C)$$
$$(A \lor B) \lor C = A \lor (B \lor C)$$

**Distributive laws**
$$A \land (B \lor C) = (A \land B) \lor (A \land C)$$
$$A \lor (B \land C) = (A \lor B) \land (A \lor C)$$

**Absorption laws**
$$A \land (A \lor B) = A \qquad A \lor (A \land B) = A$$

**Redundancy (consensus-adjacent) law**
$$A \lor (\lnot A \land B) = A \lor B$$
$$A \land (\lnot A \lor B) = A \land B$$

## 2. De Morgan's Laws

$$\lnot(A \land B) = \lnot A \lor \lnot B$$
$$\lnot(A \lor B) = \lnot A \land \lnot B$$

**Generalized (n variables):**
$$\lnot(A_1 \land A_2 \land \dots \land A_n) = \lnot A_1 \lor \lnot A_2 \lor \dots \lor \lnot A_n$$
$$\lnot(A_1 \lor A_2 \lor \dots \lor A_n) = \lnot A_1 \land \lnot A_2 \land \dots \land \lnot A_n$$

**Memory trick:** "Break the line, change the sign" — when a NOT bar breaks over AND/OR, the operator flips.

## 3. XOR (⊕) Core Identities

**Definition**
$$A \oplus B = (A \land \lnot B) \lor (\lnot A \land B)$$

**Alternative definition (useful for proofs)**
$$A \oplus B = (A \lor B) \land \lnot(A \land B)$$

**Identity/Null**
$$A \oplus 0 = A \qquad A \oplus 1 = \lnot A$$

**Self-inverse (very frequently tested)**
$$A \oplus A = 0 \qquad A \oplus \lnot A = 1$$

**Commutative**
$$A \oplus B = B \oplus A$$

**Associative**
$$(A \oplus B) \oplus C = A \oplus (B \oplus C)$$

**Own inverse property**
$$A \oplus B \oplus B = A \quad \text{(XOR-ing twice with the same value cancels out)}$$

**Distributive over AND**
$$A \land (B \oplus C) = (A \land B) \oplus (A \land C)$$

**XOR in terms of NOT**
$$\lnot(A \oplus B) = A \oplus \lnot B = \lnot A \oplus B \quad (\text{this is XNOR})$$

## 4. XOR–Absorption Style Identities (exam favorites)

$$A \oplus (A \land B) = A \land \lnot B$$
$$A \oplus (A \lor B) = \lnot A \land B$$
$$(A \land B) \oplus (A \lor B) = A \oplus B$$
$$A \lor (A \oplus B) = A \lor B$$
$$A \land (A \oplus B) = A \land \lnot B$$

## 5. XNOR (Equivalence, ↔ or ⊙)

$$A \odot B = \lnot(A \oplus B) = (A \land B) \lor (\lnot A \land \lnot B)$$

**Properties (mirror AND's structure):**
$$A \odot A = 1 \qquad A \odot \lnot A = 0$$
$$A \odot 1 = A \qquad A \odot 0 = \lnot A$$

## 6. NAND / NOR (Universal Gates) — De Morgan connection

$$A \uparrow B \;(\text{NAND}) = \lnot(A \land B) = \lnot A \lor \lnot B$$
$$A \downarrow B \;(\text{NOR}) = \lnot(A \lor B) = \lnot A \land \lnot B$$

**Expressing basic gates using only NAND:**
$$\lnot A = A \uparrow A$$
$$A \land B = (A \uparrow B) \uparrow (A \uparrow B)$$
$$A \lor B = (A \uparrow A) \uparrow (B \uparrow B)$$

## 7. Worked Example Proof (style you'll be asked to reproduce)

**Prove:** $(A \oplus B) \oplus A = B$

$$= A \oplus (B \oplus A) \quad \text{[Associative]}$$
$$= A \oplus (A \oplus B) \quad \text{[Commutative]}$$
$$= (A \oplus A) \oplus B \quad \text{[Associative]}$$
$$= 0 \oplus B \quad \text{[Self-inverse: } A\oplus A = 0\text{]}$$
$$= B \quad \text{[Identity law]}$$

## Quick Exam Tips
- Most XOR proofs reduce to using **self-inverse** ($A \oplus A = 0$) plus **associative/commutative** rearranging.
- If a proof looks stuck, try **converting XOR to its AND/OR/NOT definition** first, then apply De Morgan's + distributive laws.
- De Morgan's proofs almost always start by "pushing the NOT inward" one level at a time.
- Always double check with a **truth table** for 2–3 variables if you're unsure — fastest way to verify before writing the algebraic proof.

Want me to also include a worked example applying De Morgan's twice on a nested expression like $\lnot(A \land (B \lor \lnot C))$?



Here's the worked example, done step-by-step the way you'd write it in an exam.

## Simplify: $\lnot(A \land (B \lor \lnot C))$

**Step 1: Apply De Morgan's Law (outer level)**

The outermost structure is $\lnot(X \land Y)$ where $X = A$ and $Y = (B \lor \lnot C)$.

Using $\lnot(X \land Y) = \lnot X \lor \lnot Y$:

$$\lnot(A \land (B \lor \lnot C)) = \lnot A \lor \lnot(B \lor \lnot C)$$

**Step 2: Apply De Morgan's Law again (inner level)**

Now focus on the second term: $\lnot(B \lor \lnot C)$.

This is of the form $\lnot(X \lor Y)$ where $X = B$ and $Y = \lnot C$.

Using $\lnot(X \lor Y) = \lnot X \land \lnot Y$:

$$\lnot(B \lor \lnot C) = \lnot B \land \lnot(\lnot C) = \lnot B \land C$$

(using double negation: $\lnot(\lnot C) = C$)

**Step 3: Combine both results**

$$\lnot(A \land (B \lor \lnot C)) = \lnot A \lor (\lnot B \land C)$$

## Final Answer:
$$\boxed{\lnot A \lor (\lnot B \land C)}$$

## Verification via Truth Table (quick sanity check)

| A | B | C | B∨¬C | A∧(B∨¬C) | LHS: ¬(A∧(B∨¬C)) | ¬B∧C | RHS: ¬A∨(¬B∧C) |
|---|---|---|------|----------|-------------------|------|------------------|
| 0 | 0 | 0 | 1 | 0 | 1 | 0 | 1 |
| 0 | 0 | 1 | 1 | 0 | 1 | 1 | 1 |
| 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 1 | 1 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 |

**LHS and RHS columns match on every row** ✓ — confirms the simplification is correct.

## Key Pattern to Remember for Exams

When De Morgan's is applied to a **nested** expression:
1. Work from **outside in** — identify the outermost operator first (AND or OR) and flip it, distributing the NOT to each operand.
2. Whenever a NOT lands on a sub-expression that itself has AND/OR inside it, **repeat the process** on that sub-expression.
3. Simplify any double negations ($\lnot\lnot X = X$) as you go.
4. Stop when every NOT is directly on a single variable (not on a compound expression) — this is called **fully pushing the negation inward**.

This "push the NOT inward one layer at a time" approach works no matter how deeply nested the expression is — just keep repeating Steps 1–3 for each layer.












