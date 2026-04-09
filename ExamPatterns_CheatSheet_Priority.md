# Exam Pattern Analysis + Prioritized Cheat Sheet

Scope used: `Algos.md`, `SolutionsAltklausuren.md` (primary), and secondarily `SolutionsHA.md`, `SolutionsPrev.md`, `StudyGuide.md`.

---

## 1) Patterns in old exams (what is repeatedly asked)

## 1.1 Macro-structure pattern (very recurring)

Across multiple old exams, a repeated structure appears:

1. **Short rapid-fire block early** (often Aufgabe 1):
   - asymptotics / O-notation truth checks,
   - recurrences + Master theorem,
   - quick true/false conceptual checks,
   - scenario-to-data-structure mapping.
2. **One or more core algorithm design/tracing tasks**:
   - graph (MST, shortest paths, flow),
   - DP recurrence + base cases + runtime,
   - data structure operations by hand (heap/AVL/hash).
3. **One proof/correctness/counterexample element**:
   - induction,
   - contradiction,
   - greedy correctness or greedy-failure counterexample,
   - amortized argument.
4. **At least one “operational trace” task**:
   - run algorithm step-by-step on a concrete input.

Practical implication: prepare for **mixed format**, not only coding or only theory.

## 1.2 Topic-frequency pattern in old exams (high confidence)

High-repeat clusters (from `SolutionsAltklausuren.md`):

- **Asymptotics + recurrences + Master theorem**
- **Graph algorithms**: Dijkstra / MST (Kruskal, Prim, Borůvka) / flow
- **DP tasks**: bookshelf-style, rod-cutting, graph DP variants
- **Amortized analysis**: dynamic array, stack/queue transformations
- **Core data structures**: heaps, AVL, hashing (incl. probing)
- **Stable matching** (multiple years/probeklausuren)

Medium-repeat / special-appearance clusters:

- FFT/DFT/Karatsuba/Strassen-type divide-and-conquer numeric tasks
- randomized expected-time arguments
- special design tasks (celebrity, majority, sliding-window variants)

## 1.3 Typical exercise style pattern

Repeated grading pattern is:

- **(a)** model/define state/invariant,
- **(b)** give/derive recurrence or algorithm,
- **(c)** prove correctness (or provide counterexample),
- **(d)** runtime proof,
- **(e)** optional trace or reconstruction of actual solution path.

This means partial credit is usually available if your **state + recurrence + runtime** are right even with minor trace mistakes.

---

## 2) Patterns likely to become good exam questions (from HA/Prev/StudyGuide/Algos)

These are highly “exam-compatible” because they match recurring old-exam style and current prep emphasis.

1. **Hashing deep-dive** (open addressing vs double hashing vs cuckoo, tombstones)
2. **AVL deletion + double rotations** (not only insertion)
3. **QuickSelect trace + expected runtime intuition**
4. **Greedy vs DP contrast** (coin change failure, when greedy works)
5. **Master theorem + non-standard recurrence case detection**
6. **MST correctness laws** (cut/cycle property) + Prim/Kruskal compare
7. **Flow residual reasoning** (augmenting path, bottleneck, min-cut link)
8. **Stable matching properties** (termination + stability + proposer-optimality)
9. **Coding comparison** (Huffman optimal, Shannon-Fano not guaranteed optimal)
10. **DP state design under constraints** (interval coverage/bookshelf/LPS = Longest Palindromic Subsequence)

---

## 3) Compact exam-question bank with short solutions (high-yield)

## Q1) Why can Dijkstra fail with negative edges?
**Short solution:** Dijkstra finalizes nodes greedily (“sealed”). Negative edges can later produce a shorter path to an already-finalized node, violating correctness.

## Q2) What must be in a DP answer to get full points?
**Short solution:** state definition, transition equation, base cases, fill order/dependency order, runtime + space.

## Q3) Why does Gale-Shapley terminate in O(n^2)?
**Short solution:** each proposer can propose to each acceptor at most once; at most n² proposals.

## Q4) Why is result proposer-optimal in Gale-Shapley?
**Short solution:** no proposer can be matched to a better stable partner without creating a blocking pair under proposer-proposing deferred acceptance.

## Q5) Why does Kruskal need Union-Find?
**Short solution:** to test cycle-creation efficiently (same component?) while scanning edges in increasing weight.

## Q6) Give one canonical greedy failure pattern.
**Short solution:** coin system where local largest-coin choice is suboptimal (e.g., 1,3,4 for target 6: greedy gives 4+1+1, optimal 3+3).

## Q7) Huffman vs Shannon-Fano in one line?
**Short solution:** Huffman is optimal prefix code for known frequencies; Shannon-Fano is heuristic and can be suboptimal.

## Q8) Dynamic array amortized insert why O(1)?
**Short solution:** doubling causes occasional O(n) copy, but total copied elements over many inserts is geometric; average per insert is constant.

## Q9) Residual edge meaning in max-flow?
**Short solution:** it encodes possible extra push forward and possible cancellation/backtrack of already-sent flow.

## Q10) QuickSelect expected O(n) intuition?
**Short solution:** each partition is linear, and only one side recurses; expected remaining size shrinks by constant factor.

---

## 4) CHEAT SHEET SECTION (prioritized highest -> lowest)

Use this as direct compression target for your one-sheet.

## P0 (must fit first)

1. **Recurrence protocol**
   - Match `T(n)=aT(n/b)+f(n)`.
   - Compare `f(n)` with `n^(log_b a)`.
   - Case 1/2/3 + regularity check for case 3.
   - If not fit: recursion tree.

2. **Graph essentials**
   - Dijkstra requires nonnegative edges.
   - Kruskal: sort edges + Union-Find.
   - Prim: grow frontier from one tree.
   - Flow: residual graph + augmenting paths; stop => max flow; min-cut value equals max-flow.

3. **DP essentials template**
   - State, transition, base, order, reconstruction.
   - Common pitfalls: wrong indexing, missing infeasible-state handling.

4. **Greedy sanity check**
   - Prove exchange/cut property OR provide counterexample.
   - Coin-change warning: greedy not always optimal.

5. **Data-structure mechanics**
   - Heap index formulas.
   - Hash probing with `EMPTY` vs `DELETED`.
   - AVL balance factor in {-1,0,1}; LL/RR/LR/RL rotations.

## P1 (next layer if space)

### 4.1 Algorithms + data structures (full coverage list by Algos.md numbering)

**A) Data Structures**
1. Binary Heap
2. Priority Queue
3. Bucket Queue
4. Dynamic Array
5. Stack
6. Queue
7. Hash Table / Hash Map
8. Open Addressing (Linear Probing / Double Hashing)
9. Cuckoo Hashing
10. Universal / 2-Universal Hash Family
11. BST
12. AVL Tree
13. Union-Find
14. Residual Network (flow DS)
15. Prefix-Free Code Tree

**B) Core Graph / Flow Algorithms**
16. BFS
17. DFS
18. Connected Components
19. Dijkstra
20. Bellman-Ford
21. Floyd-Warshall
22. Kruskal MST
23. Prim MST
24. Boruvka MST
25. Topological Sort (Kahn)
26. Ford-Fulkerson
27. Edmonds-Karp
28. Cycle-Canceling in Flows
29. Bipartite Matching via Max Flow

**C) Sorting / Selection / Search**
30. Merge Sort
31. QuickSort
32. QuickSelect
33. Selection Sort
34. Insertion Sort
35. Counting Sort
36. Radix Sort (incl. MSD)
37. 2/3 Sort
38. Binary Search
39. Sequential Search
40. Two-Pointer Technique
41. Sliding Window
42. Boyer-Moore Majority Vote
43. Celebrity Problem

**D) Divide-and-Conquer / Numeric**
44. DFT
45. FFT
46. Karatsuba
47. Strassen

**E) Dynamic Programming / Greedy**
48. Matrix Chain Multiplication
49. LCS
50. Hirschberg
51. LPS
52. 0/1 Knapsack
53. Fractional Knapsack
54. Rod Cutting
55. Coin Change
56. Interval Scheduling
57. Interval Coverage DP
58. Bookshelf DP

**F) Coding / Matching / Randomized / Specialized**
59. Huffman Coding
60. Shannon-Fano Coding
61. Gale-Shapley
62. MinHash
63. Jaccard Similarity
64. Von Neumann Extractor
65. Rejection Sampling

### 4.2 High-probability exam-style prompts (with micro-answer)

- **“Design DP for X with constraints”** -> define prefix/interval state + transition by last decision.
- **“Prove greedy correctness”** -> exchange argument or cut property.
- **“Show greedy fails”** -> minimal counterexample + explicit better solution.
- **“Trace algorithm by hand”** -> table each iteration, preserve invariants.
- **“Runtime proof”** -> loop bound or recurrence + theorem/case.
- **“Amortized proof”** -> aggregate or potential method with operation sequence.

## P2 (if still space)

- Huffman merge rule and expected code-length formula.
- Shannon-Fano caveat (“not always optimal”).
- MinHash estimator idea: probability of equal min-hash equals Jaccard.
- Quick “proof templates”: induction skeleton, contradiction skeleton, loop invariant 4-step checklist.
- Exam execution strategy:
  1) secure rapid-fire points first,
  2) do one full trace carefully,
  3) for proofs, write claim + invariant/argument + conclusion explicitly.

---

## 5) Final practical takeaway

If time is limited, optimize for this order:

1. **Master theorem + asymptotics quick checks**
2. **Dijkstra/Kruskal/Prim/Flow residual mechanics**
3. **DP template + one hard DP (bookshelf/coverage/LPS)**
4. **Hashing + AVL operational traces**
5. **Stable matching + greedy-vs-DP counterexample discipline**

That sequence best matches both old-exam repetition and high-point reliability.
