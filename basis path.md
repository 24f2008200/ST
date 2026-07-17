"Basis path" comes from **McCabe's Basis Path Testing**, a white-box testing technique built on the control flow graph (CFG) — the same kind of graph you've been working with (DD-paths, decision vertices, etc.).

## The core idea

In a CFG with loops and branches, there can be an infinite number of possible paths (because of loops) or a very large number (because of nested decisions). Testing *every* path is impractical. McCabe's insight: you don't need every path — you need a minimal set of paths that, together, can generate (by linear combination) every other path in the graph. This minimal set is called the **basis set**, and each path in it is a **basis path**.

Think of it like a basis in linear algebra: a small set of independent vectors that can combine to reach any point in the space. Basis paths are the "independent vectors" of the control flow graph.

## How many basis paths do you need?

Exactly as many as the **cyclomatic complexity**, V(G), of the graph:

$$V(G) = E - N + 2$$

where E = number of edges, N = number of nodes (using the CFG, not the DD-path graph). Equivalently, V(G) = number of decision points + 1.

This number tells you both:
- The number of independent paths (= number of test cases you need)
- An upper bound on the number of linearly independent circuits/regions in the graph

## Steps in Basis Path Testing

1. **Draw the CFG** from the code/flowchart.
2. **Calculate V(G)** using the formula above.
3. **Identify the basis set of V(G) independent paths** — each new path must traverse at least one edge not covered by previous paths.
4. **Derive test cases** for each of these paths, choosing input data that forces execution along that specific path.

## Quick example using your graph

If we treat your graph (1→2→3→4→6, 2→5→6) as a mini CFG:
- E = 6, N = 6
- V(G) = 6 − 6 + 2 = **2**

That matches the 2 decision-driven routes you found earlier:
- Path A: 1-2-3-4-6
- Path B: 1-2-5-6

So here, the basis paths are simply these two paths — every other "path" in this particular graph is just these two (there's no loop, so no linear combinations needed beyond the two branches).

## Why it matters

- **Guarantees each edge is executed** at least once (a weaker goal — statement coverage) and, more importantly, ensures **each decision outcome** is exercised in some independent path (branch/decision coverage).
- It's more rigorous than arbitrary path selection because the paths are **linearly independent** — no path in the basis can be expressed as a combination of the others, so you avoid redundant tests while still achieving thorough control-flow coverage.
- It's especially useful when loops are involved, since it avoids the "infinite paths" problem — you just need enough paths to cover the independent regions, not every possible loop iteration count.

If you want, I can walk through a basis path example with a loop in the CFG, since that's where the technique really shows its value.
