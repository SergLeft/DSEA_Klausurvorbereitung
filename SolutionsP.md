# Solutions and Theory for Präsenzblätter

## 🏗 Topic 1: Heaps (The Priority Engine)

### 🧠 The Theory: Why and What?
Imagine an **Emergency Room (ER)**. Patients aren't treated in the order they arrive; they are treated by **Priority**.
- **The Problem**: A sorted list is slow for adding patients ($O(n)$). An unsorted list is slow for finding the most urgent one ($O(n)$).
- **The Solution (Heap)**: A binary tree that stays "mostly sorted" so both adding and finding the maximum take only $O(\log n)$ time.
- **The Structure**: A **Complete Binary Tree** (no gaps, filled level-by-level). Because it's "complete," we can store it in a simple **Array**:
  - Left Child: $2i + 1$
  - Right Child: $2i + 2$
  - Parent: $\lfloor (i-1)/2 \rfloor$
- **The "Law" (Heap Property)**: In a **Max-Heap**, every parent must be $\ge$ its children. Note: This doesn't mean the whole tree is sorted! It just means that any path from root to leaf is in descending order.

### 🛠 The Mechanics: Trickle-Down (Sift-Down)
When the "King" (Max value) is replaced by a "Peasant" (small value), the heap property is broken.  
- **The Fix**: The Peasant compares himself with his children. He swaps with the **largest** child. He repeats this until he is larger than his children or hits the bottom.  
- **Why the largest?** If you swap with the smaller child, the larger child would still be bigger than the new parent, breaking the law again!

### 🚀 Floyd’s Algorithm (Bottom-Up Heap Building)
- **The Strategy**: Throw all numbers into an array randomly. Start at the last non-leaf node ($\lfloor n/2 \rfloor - 1$) and work backwards to the root, calling `trickle-down` on each.
- **The Efficiency ($O(n)$)**: Most nodes are at the bottom and move 0 or 1 steps. Only the root moves $\log n$. The "lazy majority" at the bottom makes the total work linear.
- **Formal Proof**:
  - Nodes at height $h$ do at most $h$ swaps.
  - Number of nodes at height $h \approx n/2^{h+1}$.
  - Total Work $S = \sum \frac{n}{2^{h+1}} \cdot h = \frac{n}{2} \sum \frac{h}{2^h}$.
  - The series $\sum \frac{h}{2^h}$ converges to 2.
  - Total $S \approx \frac{n}{2} \cdot 2 = O(n)$.

#### ✅ Solution 1b: Step-by-Step Trace
**Initial Array:** `[4, 1, 5, 5, 2, 6, 7, 3, 4, 6, 8, 3]`
1. **$i=5$ (Val 6)**: $6 \ge 3$. OK.
2. **$i=4$ (Val 2)**: Children are 6 and 8. **Swap 2 and 8.**
3. **$i=3$ (Val 5)**: $5 \ge \max(3, 4)$. OK.
4. **$i=2$ (Val 5)**: Children are 6 and 7. **Swap 5 and 7.**
5. **$i=1$ (Val 1)**: Children are 5 and 8. **Swap 1 and 8.** Trickle 1 down: Children are 6 and 2. **Swap 1 and 6.**
6. **$i=0$ (Val 4)**: Children are 8 and 7. **Swap 4 and 8.** Trickle 4 down: Children are 5 and 6. **Swap 4 and 6.** Trickle 4 down: Children are 1 and 2. OK.
**Final Max-Heap:** `[8, 6, 7, 5, 4, 6, 5, 3, 4, 1, 2, 3]`

---

## 🌊 Topic 2: Flow Networks (The Plumbing System)

### 🧠 The Theory: Pipes and Flow
Imagine water pipes from Source ($s$) to Sink ($t$).
- **Capacity ($c$)**: The max liters per second a pipe can hold.
- **Flow ($f$)**: The actual water volume currently moving through.
- **Law of Conservation**: At every junction (except $s$ and $t$), water in must equal water out.
- **Residual Network ($G_f$)**: The "Possibility Map."
  - **Forward Edge**: How much *more* can we send? ($c - f$).
  - **Backward Edge**: How much can we *cancel* or redirect? ($f$).
- **Augmenting Path**: A valid route from $s$ to $t$ in the Residual Network where all capacities $> 0$.
- **Bottleneck**: The smallest residual capacity on a path. It limits the total flow increase for that path.

#### ✅ Solution Blatt 13, Aufgabe 2
**Path Found**: $s \to 4 \to 5 \to t$.
**Residual Capacities**: $14, 5, 9$.
**Bottleneck**: $5$. We can increase total flow by **5 units**.

---

## 🌲 Topic 3: MST & Traversals (The Network Designer)

### 🧠 The Theory: Connecting Cities
- **MST (Minimum Spanning Tree)**: Connect all cities with the minimum total cable length. No cycles allowed (cycles waste money).
- **Prim's Algorithm**: "The Empire." Start at one city and always add the cheapest road connecting your existing territory to a new city.
- **Kruskal's Algorithm**: "The Contractor." Look at all possible roads. Build the cheapest one available unless it creates a cycle.
- **DFS (Depth-First)**: "The Explorer." Go as deep as possible down a path, then backtrack. Uses a **Stack**. Great for cycle detection.
- **BFS (Breadth-First)**: "The Wave." Visit all immediate neighbors, then their neighbors. Uses a **Queue**. Great for finding the shortest path in unweighted graphs.
- **Connected Components**: Isolated groups of nodes ("Islands"). Found by running DFS/BFS until every node is visited.

#### ✅ Solution Blatt 12, Aufgabe 1 (MST)
**Sorted Edges**: (C-D: 0), (D-E: 1), (B-E: 2), (A-B: 3), (C-G: 4), (G-H: 5), (E-G: 6 - CYCLE), (B-F: 7).
**Final MST Weight**: 22.

---
*Status: Reviewing Blatt 13 & 12. Ready for Blatt 11 when you are.*