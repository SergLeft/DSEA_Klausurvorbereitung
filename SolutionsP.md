# Solutions and Theory for Präsenzblätter

## Präsenzblatt 13

### Aufgabe 1: Heaps (Max-Heap)

#### 🧠 Theory: What is a Heap?
A **Heap** is a complete binary tree used for priority queues.
*   **Max-Heap Property**: For every node $i$, $Value(Parent(i)) \ge Value(i)$. The largest value is always at the root.
*   **Array Representation**: 
    *   Index of Left Child: $2i + 1$
    *   Index of Right Child: $2i + 2$
    *   Index of Parent: $\lfloor (i-1)/2 \rfloor$

#### 🚀 Building a Heap in $O(n)$ (Floyd's Algorithm)
Instead of $n$ insertions ($O(n \log n)$), we use a bottom-up approach:
1.  Start at the last non-leaf node: $i = \lfloor n/2 \rfloor - 1$.
2.  Work backwards to index 0, calling `trickle-down` (sift-down) on each node.

#### 📝 Aufgabe 1a: Formal Proof of $O(n)$
**Question:** Why is building a heap bottom-up $O(n)$ when each `trickle-down` can take $O(\log n)$?
**Proof:**
1.  **Node Distribution:** In a perfect binary tree of height $H$, there are at most $\lceil n/2^{h+1} \rceil$ nodes at height $h$. (Leaves at $h=0$ comprise $\approx n/2$ nodes).
2.  **Work per Height:** A node at height $h$ can "trickle down" at most $h$ levels.
3.  **Total Work ($S$):** 
    $$S = \sum_{h=0}^{\log n} \frac{n}{2^{h+1}} \cdot h = \frac{n}{2} \sum_{h=0}^{\log n} \frac{h}{2^h}$$
4.  **Convergence:** Using the series $\sum_{k=0}^{\infty} kx^k = \frac{x}{(1-x)^2}$ with $x = 1/2$:
    $$\sum_{h=0}^{\infty} h(\frac{1}{2})^h = 2$$
5.  **Conclusion:** $S \approx \frac{n}{2} \cdot 2 = n$. Result: **$O(n)$**.

#### ✅ Solution 1b: Step-by-Step
**Initial Array:** `[4, 1, 5, 5, 2, 6, 7, 3, 4, 6, 8, 3]` ($n=12$)
1.  **$i=5$ (Val 6):** $6 \ge 3$. OK.
2.  **$i=4$ (Val 2):** Max(2, 6, 8) is 8. **Swap 2 and 8.**
3.  **$i=3$ (Val 5):** $5 \ge \max(3, 4)$. OK.
4.  **$i=2$ (Val 5):** Max(5, 6, 7) is 7. **Swap 5 and 7.**
5.  **$i=1$ (Val 1):** Max(1, 5, 8) is 8. **Swap 1 and 8.** Then trickle 1 down: Max(1, 6, 2) is 6. **Swap 1 and 6.**
6.  **$i=0$ (Val 4):** Max(4, 8, 7) is 8. **Swap 4 and 8.** Then trickle 4 down: Max(4, 5, 6) is 6. **Swap 4 and 6.**
**Final Max-Heap:** `[8, 6, 7, 5, 4, 6, 5, 3, 4, 1, 2, 3]`

---

### Aufgabe 2: Residualnetzwerke & Fluss

#### 🧠 Theory: Residual Network ($G_f$)
Shows how much flow we can still push through a network.
*   **Forward Edge ($u \to v$):** Capacity = $c(u,v) - f(u,v)$. (How much more can we send?)
*   **Backward Edge ($v \to u$):** Capacity = $f(u,v)$. (How much can we "send back" or cancel?)

#### ✅ Solution
**Augmenting Path found:** $s \to 4 \to 5 \to t$.
*   Residual capacities: $s \xrightarrow{14} 4 \xrightarrow{5} 5 \xrightarrow{9} t$.
*   **Bottleneck:** $\min(14, 5, 9) = 5$.
*   We can increase the total flow by **5 units**.

---

## Präsenzblatt 12

### Aufgabe 1: MST (Prim & Kruskal)

#### 🧠 Theory: MST
A subset of edges connecting all nodes with minimum total weight and no cycles.
*   **Kruskal**: Greedy choice of edges. Sort edges by weight and pick if no cycle is formed.
*   **Prim**: Growing tree. Start at node A, pick cheapest edge connecting the tree to a new node.

#### ✅ Solution
**Edges:** (A-B: 3), (A-C: 8), (A-D: 10), (B-E: 2), (B-F: 7), (C-D: 0), (D-E: 1), (E-F: 11), (C-G: 4), (E-G: 6), (F-H: 9), (G-H: 5)

**Kruskal Steps:**
1. (C-D, 0) - OK
2. (D-E, 1) - OK
3. (B-E, 2) - OK
4. (A-B, 3) - OK
5. (C-G, 4) - OK
6. (G-H, 5) - OK
7. (B-F, 7) - OK
**Total Weight: 22**

### Aufgabe 2: Connected Components (ZHKs)
**Theory**: To find all connected components in $O(V+E)$, use DFS or BFS.
1. Start at an unvisited node, mark as visited, and perform a traversal to find all reachable nodes (one component).
2. Repeat for all remaining unvisited nodes.
3. Each traversal start marks a new component.