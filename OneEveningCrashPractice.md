# One-Evening Crash Order — Practice Pack

Use this right after the crash-order list in `Algos.md`.

---

## 1) Recurrences

1. Solve with Master theorem (state case + final bound):
   - `T(n)=2T(n/2)+n`
   - `T(n)=3T(n/2)+n`
   - `T(n)=2T(n/2)+n log n`
2. Solve one non-standard recurrence via recursion tree/substitution:
   - `T(n)=T(2n/3)+n`

**Self-check:** Did you justify *why* the theorem/case applies, not just state the result?

---

## 2) Heap + Priority Queue Mechanics

Given array-based min-heap:
`[2, 4, 7, 9, 11, 15, 20]`

1. Insert `3` and show each sift-up swap.
2. Extract-min once and show each sift-down swap.
3. Explain why each operation is `O(log n)`.

**Self-check:** Parent/child index math correct (`parent=(i-1)//2`, children `2i+1,2i+2`)?

---

## 3) Dijkstra + Shortest-Path Traps

Graph (directed, nonnegative):
- `s->a (2), s->b (5), a->b (1), a->c (4), b->c (1), c->t (3)`

1. Run Dijkstra from `s` and record PQ states.
2. Final distances + predecessor tree.
3. State exactly why negative edges break Dijkstra.

**Self-check:** Every relaxation done only when it improves `dist[v]`?

---

## 4) MST Package: Kruskal + Prim + Union-Find

Undirected weighted graph:
- `ab:1, ac:4, bc:2, bd:5, cd:1, ce:3, de:2`

1. Run Kruskal (sorted edges + union-find decisions).
2. Run Prim from `a`.
3. Verify both give same MST weight.

**Self-check:** Did you reject edges that create cycles during Kruskal?

---

## 5) Hashing Package

Table size `m=11`, `h1(k)=k mod 11`, linear probing.
Insert keys: `22, 1, 13, 11, 24, 35`.

1. Show final table with probe traces.
2. Delete one colliding key using tombstone.
3. Search a key beyond the tombstone and explain why search still works.
4. State how double hashing changes probe sequence.

**Self-check:** You never stop search at `DELETED`, only at `EMPTY`.

---

## 6) AVL Essentials

Insert in order: `30, 20, 10, 25, 40, 50, 22`.

1. Draw tree after each insertion.
2. Identify and name each rotation type used (LL, RR, LR, RL).
3. Delete one node that triggers rebalancing and show fix.

**Self-check:** Height/balance factors updated bottom-up?

---

## 7) Dynamic Programming Core Pattern

For each problem, write **state**, **transition**, **base case**, **answer extraction**:
1. 0/1 Knapsack
2. LCS
3. Matrix Chain Multiplication

**Self-check:** Is each transition only using already-computed subproblems?

---

## 8) Greedy vs DP Comparison

1. Give coin system where greedy fails for minimum coins.
2. Show greedy choice sequence.
3. Show better optimal solution and why DP finds it.

**Self-check:** Counterexample must be valid (same target, fewer coins than greedy).

---

## 9) Flow Basics

Network:
- `s->a:3, s->b:2, a->b:1, a->t:2, b->t:3`

1. Build residual graph initially.
2. Perform augmenting paths until max flow.
3. Show final max flow value and one minimum cut.

**Self-check:** Residual reverse edges updated after each augmentation?

---

## 10) Stable Matching (Gale-Shapley)

Create 3 proposers + 3 acceptors with strict preferences.

1. Do full hand trace of proposals.
2. Give final matching.
3. Argue why no blocking pair exists.

**Self-check:** No proposer can propose to same acceptor twice.

---

## 11) QuickSelect / Partition

Array: `[12, 3, 5, 7, 4, 19, 26]`, find 4th smallest.

1. Run one full partition step (show pivot placement).
2. Recurse only relevant side.
3. Explain expected `O(n)` intuition.

**Self-check:** Are you adjusting `k` correctly when recursing right?

---

## 12) Final 30-Min Mixed Drill

Do this under time pressure:
- **2 tracing tasks** (one graph, one data structure)
- **2 runtime tasks** (derive and justify)
- **1 proof-style task** (invariant or correctness argument)

Suggested picks:
- Trace: Dijkstra + Open Addressing
- Runtime: MergeSort + QuickSelect
- Proof: Gale-Shapley stability or Kruskal cut property

---

## Rapid Scoring (Optional)

- 0/1 for each section completed with clean logic.
- Target: **10+/12** before exam day.
- Redo only missed sections immediately.
