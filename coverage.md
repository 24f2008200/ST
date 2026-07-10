To understand **why All Uses Coverage (AUC) subsumes All Defs Coverage (ADC)** and **why All Uses Coverage subsumes Edge Coverage (EC)**, it helps to compare what each criterion requires.

---

# 1. How All Uses Coverage subsumes All Defs Coverage

## Definitions

### All Defs Coverage (ADC)

For **every definition** of a variable, execute **at least one** def-clear path from that definition to **some use** (either c-use or p-use).

Requirement:

> Every definition → at least one reachable use.

---

### All Uses Coverage (AUC)

For **every definition**, execute a def-clear path to **every reachable use**.

Requirement:

> Every definition → every reachable use.

---

## Example

```
      1
      x = 5      (def x)
       |
       v
      2
   if (a>0)      (p-use x)
    /     \
   /       \
  3         4
y=x+1     z=x+2   (c-use x)
   \       /
    \     /
      5
```

Definition:

```
Node 1 : def(x)
```

Uses:

```
Node 2 : p-use(x)
Node 3 : c-use(x)
Node 4 : c-use(x)
```

---

### All Defs Coverage requires

Only one of these is enough:

```
1 → 2
```

or

```
1 → 3
```

or

```
1 → 4
```

Once one reachable use has been exercised, ADC is satisfied.

---

### All Uses Coverage requires

All reachable uses:

```
1 → 2
1 → 3
1 → 4
```

---

Since AUC already includes **at least one use for every definition**, it automatically satisfies ADC.

Hence

[
\boxed{\text{All Uses Coverage } \supseteq \text{ All Defs Coverage}}
]

or

[
\boxed{\text{AUC subsumes ADC}}
]

---

# Intuition

Think of a teacher giving homework.

ADC says:

> "For every chapter, solve **one** problem."

AUC says:

> "For every chapter, solve **every** problem."

If you've solved every problem, you've obviously solved one problem from each chapter.

---

# 2. How All Uses Coverage subsumes Edge Coverage

This is slightly less obvious.

---

## Edge Coverage

Requirement:

Every edge in the control-flow graph must be traversed at least once.

Example

```
     1
     |
     v
     2
   /   \
  v     v
 3       4
  \     /
   \   /
     5
```

Edges are

```
1→2
2→3
2→4
3→5
4→5
```

Edge Coverage requires all five edges.

---

## Why does All Uses Coverage force edge traversal?

Suppose

```
1: x = 10
      |
      v
2: if(...)
   /     \
  v       v
3: print(x)
4: print(x)
```

Definition:

```
Node 1 : def(x)
```

Uses:

```
Node 3
Node 4
```

To reach both uses:

Need

```
1→2→3
```

and

```
1→2→4
```

Notice what happened.

To satisfy AUC, you automatically traversed

```
1→2
2→3
2→4
```

Those are edges.

---

## Another example

```
      1
      |
      v
      2
      |
      v
      3
```

```
Node 1 : def(x)
Node 3 : use(x)
```

To satisfy AUC:

```
1→2→3
```

Automatically traverses

```
1→2
2→3
```

So edge coverage is also satisfied.

---

## Why is this always true?

Every use is reached through some path.

That path is made of edges.

Since AUC requires executing paths from every definition to every reachable use, **every edge that lies on those required def-use paths gets traversed**.

Under the standard assumptions used in data-flow testing (where the graph is well formed and executable code is associated with definitions/uses), the required def-use paths collectively exercise every executable edge. Therefore:

[
\boxed{\text{All Uses Coverage } \supseteq \text{ Edge Coverage}}
]

---

# Visual hierarchy

```
                 All DU Paths
                      │
                      ▼
              All Uses Coverage
                 /          \
                ▼            ▼
      All Defs Coverage   Edge Coverage
```

So:

* ✅ **AUC subsumes ADC** because covering **every use** certainly covers **at least one use** for each definition.
* ✅ **AUC subsumes EC** because reaching every required def-use pair necessarily traverses the control-flow edges needed to get to those uses, ensuring every executable edge is exercised.
