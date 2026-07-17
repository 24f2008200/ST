In FSM-based testing, the reason data flow coverage typically models only a **subset of variables** — rather than every variable in the program — comes down to keeping the model tractable. Here's the reasoning:

## The problem: state space explosion

An FSM's states are meant to represent meaningful configurations of the system. If you tried to fold *every single program variable* into the state definition, the number of states would blow up combinatorially — each new variable multiplies the number of possible state combinations. A system with even a handful of variables, each with a range of values, quickly becomes computationally and practically unmanageable to model, let alone test exhaustively.

## The solution: pick the variables that matter

Instead, when applying data flow coverage to an FSM, testers select a **subset of variables that are relevant to the properties being tested** — typically the ones involved in meaningful **def-use pairs** (a variable *defined* in one place and *used* in another, potentially in a different state or transition). These are usually:

- Variables that control state transitions (decision variables)
- Variables whose incorrect propagation would cause a real fault (e.g., a flag, a counter, a critical data value)
- Variables with non-trivial def-use chains spanning multiple states/transitions

Variables that are purely local, temporary, or don't affect flow of control or critical outputs are abstracted away and left out of the FSM model entirely.

## Why this still gives good coverage

This mirrors the same philosophy as basis path testing (which we just discussed) — you don't need to test everything exhaustively; you need to cover the **paths/associations that actually matter** for revealing faults. By narrowing to the subset of variables with meaningful def-use associations:

- The **FSM stays a manageable size** (states = meaningful configurations, not exhaustive combinations)
- You still achieve **data flow coverage criteria** (all-defs, all-uses, all-du-paths) *for the variables that matter*
- Testing effort is focused where **faults are most likely to hide** — in the interactions between definitions and uses across states — rather than diluted across irrelevant variables

## Analogy

It's similar to why you don't model every possible input value for boundary value analysis — you pick representative/critical values. Here, you don't model every variable — you pick the ones whose data flow (define → use) crosses state boundaries and could plausibly cause an incorrect transition or output if handled wrong.

If you're working through a specific example (e.g., from an assignment) where you need to identify which variables to model as part of an FSM, I'm happy to work through that with you.
