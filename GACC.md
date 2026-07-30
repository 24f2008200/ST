**Claim:** GACC (General Active Clause Coverage) does not subsume Predicate Coverage (PC).

**Counterexample:** `p = a XOR b`

For GACC, each clause must be shown to *independently determine* the predicate — i.e., there must exist tests where flipping that clause (holding the other clauses at some value, possibly different for the true-case and false-case tests) changes `p`'s outcome.

For XOR, flipping either clause **always** changes the outcome, regardless of what the other clause is set to. So determination holds for *any* choice of minor-clause value — this is exactly the flexibility GACC allows (unlike CACC/RACC, which pin the minor clauses to the same value across the two tests).

Exploit that freedom:

| Test | a | b | p = a XOR b |
|------|---|---|-------------|
| T1   | T | F | **T** |
| T2   | F | T | **T** |

- **Clause `a`:** T1 has `a=T`, T2 has `a=F`. In each, flipping `a` (with `b` held at its test value) flips `p` — so `a` is shown to determine `p` in both directions. ✅ GACC satisfied for `a`.
- **Clause `b`:** same reasoning symmetrically — T2 has `b=T`, T1 has `b=F`, and flipping `b` flips `p` in both. ✅ GACC satisfied for `b`.

So `{T1, T2}` is a valid GACC-satisfying test suite for the whole predicate.

**But** look at the actual values of `p` across the suite: it's **True in both tests** — `p` never evaluates to False. Predicate Coverage requires `p` to take both `True` and `False` at least once. That's violated.

Quick check:

```python
def p(a, b): return a ^ b

tests = [(True, False), (False, True)]
print([p(a, b) for a, b in tests])  # [True, True] -> p never False
```

**Why this happens:** GACC only requires each clause to *determine* the predicate at *some* minor-clause assignment — and that assignment is allowed to differ between the true-instance and false-instance test for a given clause (that's what makes it "General" rather than Correlated/Restricted). Nothing in the definition forces the *actual* observed predicate values across the whole suite to include both True and False; it only constrains pairwise counterfactuals per clause. XOR exploits this because determination is independent of the other clause's value, so you're free to pick minor values that keep `p` constant across the suite while still satisfying determination for each clause individually.

This is precisely why, in the standard subsumption hierarchy (Ammann & Offutt), **GACC and PC are incomparable** — neither subsumes the other — while CACC and RACC (which fix the minor clause values, removing this loophole) *do* subsume PC.

**Answer: Rows 3 and 7.**

**Why:**

A CACC (Correlated Active Clause Coverage) pair for clause **a** requires two test rows where:
- the **minor clauses** (`b`, `c`) are held at the **same** value,
- the **major clause** (`a`) differs (one T, one F),
- and the predicate `p` itself takes **different** values between the two rows (this is what makes `a` the "determining" clause).

Building the full truth table for `p = ¬c ∨ (a ∧ ¬b)`:

| # | a | b | c | p |
|---|---|---|---|---|
| 1 | T | T | T | F |
| 2 | T | T | F | T |
| 3 | T | F | T | T |
| 4 | T | F | F | T |
| 5 | F | T | T | F |
| 6 | F | T | F | T |
| 7 | F | F | T | F |
| 8 | F | F | F | T |

Row 3: `a=T, b=F, c=T` → `p=T`
Row 7: `a=F, b=F, c=T` → `p=F`

- `b` and `c` are identical across the two rows (both `b=F, c=T`) ✅ minor clauses fixed
- `a` flips from T to F ✅ major clause differs
- `p` flips from T to F ✅ predicate outcome differs

This matches the `p_a = T` entries the truth table already marks at rows 3 and 7, confirming `a` is shown to determine `p` at this specific minor-clause assignment (`b=F, c=T`) — which is exactly the CACC requirement (contrast this with GACC, which would let the minor-clause values differ between the two rows, and RACC, which requires the same pair to work simultaneously for *every* clause).




I had it right — let me lay out the actual distinction so it's clear why "same minor-clause values" belongs to CACC, not GACC.

For a major clause `ci` to "determine" predicate `p`, you need two tests where flipping `ci` flips `p` — the difference between GACC, CACC, and RACC is entirely about **what's required of the minor clauses across those two tests**:

- **GACC (General ACC):** `ci` must determine `p` in each test individually, but the minor clauses are allowed to take **different** values between the two tests (t1 and t2). This is the loosest requirement — it just needs *some* minor-clause assignment in t1 and *some* (possibly different) minor-clause assignment in t2, as long as `ci` determines `p` in each.

- **CACC (Correlated ACC):** tightens this by requiring the minor clauses to have the **same** values in both tests. You find one minor-clause assignment that makes `ci` determine `p`, then use that exact assignment for both the `ci=T` test and the `ci=F` test.

- **RACC (Restricted ACC):** tightens further — same as CACC (minor clauses fixed across the pair), but additionally requires that assignment be the *unique* one making `ci` determine `p` (the "unique true point" condition).

So the ordering from loosest to strictest is **GACC ⊆ CACC ⊆ RACC** in terms of constraint strength (and RACC ⇒ CACC ⇒ GACC in terms of subsumption — satisfying RACC gets you CACC and GACC for free).

This is actually the exact same GACC-vs-others distinction that came up in your earlier question about `a XOR b`: GACC could satisfy determination for clause `a` using *different* `b` values across the two tests, which is precisely what let it dodge Predicate Coverage. If CACC's fixed-minor-value constraint had been in play there, that particular escape wouldn't have worked.

So for the row-3/row-7 answer: `b=F, c=T` held identical in both rows, with `a` flipping and `p` flipping — that's a textbook CACC pair for `a`, not RACC. (It would also need to be checked for uniqueness to qualify as RACC, but the "minor clauses same value" part alone is CACC's defining feature, correctly.)

Good — this predicate is actually a textbook example where the three criteria genuinely diverge. Let's work it out clause by clause.

## Step 1: Full determination table

Using `p = ¬c ∨ (a ∧ ¬b)`, computing `p` and each `p_a, p_b, p_c` (whether flipping that clause alone changes `p`) for all 8 rows:

| # | a | b | c | p | p_a | p_b | p_c |
|---|---|---|---|---|-----|-----|-----|
| 1 | T | T | T | F |     | T   | T   |
| 2 | T | T | F | T |     |     | T   |
| 3 | T | F | T | T | T   | T   |     |
| 4 | T | F | F | T |     |     |     |
| 5 | F | T | T | F |     |     | T   |
| 6 | F | T | F | T |     |     | T   |
| 7 | F | F | T | F | T   |     | T   |
| 8 | F | F | F | T |     |     | T   |

This matches your uploaded table exactly.

## Step 2: Clause `a` — no subtlety

Only rows 3 and 7 have `p_a = T`. That's the *only* place `a` determines `p` at all. Minor clauses (`b=F, c=T`) happen to match automatically, and it's the unique such assignment.

**GACC = CACC = RACC = {3, 7}** — all three criteria collapse to the same single pair, because there's no freedom to exploit.

## Step 3: Clause `b` — no subtlety either

Only rows 1 and 3 have `p_b = T`, with `a=T, c=T` matching in both, and this is the only assignment that works.

**GACC = CACC = RACC = {1, 3}**

## Step 4: Clause `c` — this is where it gets interesting

`p_c = T` in *six* rows: 1, 2, 5, 6, 7, 8. Since `c` matters exactly when `a ∧ ¬b` is false, `c` determines `p` under **three different minor-clause assignments**: `(a,b) = (T,T)`, `(F,T)`, `(F,F)`.

- **GACC for `c`:** pick *any* c=T row from {1,5,7} and *any* c=F row from {2,6,8} — minor clauses don't need to match. E.g. `{1, 8}`: row1 has `(a,b)=(T,T)`, row8 has `(a,b)=(F,F)` — completely mismatched, but GACC doesn't care.

- **CACC for `c`:** now the minor clauses must match between the two rows. Valid pairs: `{1,2}` (a=T,b=T fixed), `{5,6}` (a=F,b=T fixed), or `{7,8}` (a=F,b=F fixed). You must pick one *consistent* assignment — no mixing.

- **RACC for `c`:** **impossible.** RACC requires the minor-clause assignment that makes `c` determinative to be the *unique* one. Here there are three such assignments, so no assignment is unique — RACC coverage for clause `c` cannot be achieved at all in this predicate.

## Step 5: Putting together full test suites

- **GACC suite:** `{1, 3, 7, 8}` — 4 rows (using mismatched minor clauses for `c`: row1 vs row8).
- **CACC suite:** `{1, 3, 7, 8}` — same 4 rows work, but now the `c`-pair `{7,8}` happens to have matching minors `(a=F,b=F)`, so it's valid CACC too.
- **RACC suite:** **does not exist** — no test suite can achieve RACC coverage for this predicate, because clause `c` has no unique determining assignment.

## Verification codeThe code confirms the hand analysis exactly.

**The key takeaway — the subtlety in one sentence:**

> GACC only asks "does each clause determine `p` *somewhere*, possibly under different minor-clause contexts for the true-side and false-side test?" CACC tightens this to "…*under the same* minor-clause context." RACC tightens further to "…and that context must be the *only* one that works."

For clauses `a` and `b` in this predicate, there was only ever one possible minor-clause context, so all three criteria coincided trivially. Clause `c` is the interesting case: because `c ∧ ¬b` collapsing has **three** distinct minor-clause contexts `(a,b) ∈ {(T,T), (F,T), (F,F)}` that all make `c` determinative, you get real daylight between the criteria:

| Criterion | Requirement on minors for `c` | Feasible? |
|---|---|---|
| GACC | can differ between true/false test | ✅ e.g. {1,8} |
| CACC | must match between true/false test | ✅ e.g. {1,2}, {5,6}, or {7,8} |
| RACC | must match **and** be the unique context | ❌ (3 contexts exist, none unique) |

This is the standard textbook demonstration of why RACC ⇒ CACC ⇒ GACC in strength, but also why **RACC coverage isn't always achievable** — some predicates simply don't have a "unique true point" for every clause, and this predicate is exactly such a case.

