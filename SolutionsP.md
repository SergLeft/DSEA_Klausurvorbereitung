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
3.  **Why $O(n)$?** Most nodes are near the bottom (height 0 or 1) and do very little work. Only the few nodes at the top do $O(\log n)$ work. The sum of work per node height converges to $O(n)$.

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
*   **Augmenting Path:** A path from $s$ to $t$ where all residual capacities are $> 0$.

#### ✅ Solution
**Augmenting Path found:** $s \to 4 \to 5 \to t$.
*   Residual capacities: $s \xrightarrow{14} 4 \xrightarrow{5} 5 \xrightarrow{9} t$.
*   **Bottleneck:** $\min(14, 5, 9) = 5$.
*   We can increase the total flow by **5 units**.

---
*Next: Präsenzblatt 12 (MST: Prim & Kruskal)*