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

## 🌊 Topic 2: The Plumbing System (Flow Networks)

### 🧠 The Theory: Pipes and Flow
Imagine water pipes from Source ($s$) to Sink ($t$).
- **Capacity ($c$)**: The max liters per second a pipe can hold (the diameter).
- **Flow ($f$)**: The actual water volume currently moving through.
- **Residual Network ($G_f$): The "Map of Possibilities"**
  - **Forward Arrow ($u \to v$): "How much more can I push?"** ($c - f$). If the pipe isn't full, you can send more.
  - **Backward Arrow ($v \to u$): "The Undo Button"** ($f$). If you're already sending 5 liters, you have the "possibility" of sending 5 liters *back* (by stopping the flow). This allows rerouting!
- **Augmenting Path**: A clear route from $s$ to $t$ in the "Map of Possibilities."
- **Bottleneck: "The Weakest Link"**
  The pipe with the smallest residual capacity on a path. If you try to send more than the bottleneck, the pipe "bursts."

#### ✅ Solution Blatt 13, Aufgabe 2
**Path Found**: $s \to 4 \to 5 \to t$.
**Residual capacities (Space left)**: $14, 5, 9$.
**Bottleneck**: $5$. We can increase total flow by **5 units**.

---

## 🏝 Topic 3: The Archipelago (MST & ZHKs)

### 🧠 The Theory: Connecting Islands
You have 8 islands and want to connect them all with bridges for the least money.
- **MST (Minimum Spanning Tree)**: The cheapest set of bridges that connects everyone without cycles (circles waste money).
- **Kruskal's Algorithm: "The Greedy Contractor"**
  Look at ALL possible bridges in the ocean. Pick the cheapest one. Build it unless it creates a circle.
- **Prim's Algorithm: "The Growing Empire"**
  Start at your capital island. Always add the cheapest "frontier" bridge that leads to an island you don't own yet.

#### ✅ Solution Blatt 12, Aufgabe 1 (MST)
**Contractor's (Kruskal) Order**:
1. (C-D, 0), 2. (D-E, 1), 3. (B-E, 2), 4. (A-B, 3), 5. (C-G, 4), 6. (G-H, 5), 7. (B-F, 7).
**Total Weight**: 22.

#### ✅ Solution Blatt 12, Aufgabe 2: ZHKs (The "Paint Bucket" Method)
**Problem**: Find all isolated island groups (ZHKs) in a subgraph $H$ in $O(V+E)$.
**Algorithm**:
1. Take a bucket of **Red Paint**. Land on an unvisited island.
2. Paint it Red. Follow every bridge from that island to its neighbors. Paint them Red too.
3. Keep going until you can't find any more bridges leading to unpainted islands. **This is one "Island Group" (ZHK).**
4. If there are still unpainted islands, pick a new one and a new color (**Blue Paint**). Repeat.
**Complexity**: Each island is painted once ($V$), and each bridge is crossed twice ($E$). Total: $O(V+E)$.

---

## 🔍 Topic 4: The Searchers (BFS vs. DFS)
- **DFS (Depth-First): "The Explorer"**
  Pick a path and go as deep as possible down a path, then backtrack. Uses a **Stack** (think: a stack of trays). Good for finding cycles.
- **BFS (Breadth-First): "The Search Party Wave"**
  Visit all immediate neighbors first, then their neighbors. Moves out in waves. Uses a **Queue** (think: a line at the supermarket). Good for finding the shortest path in unweighted graphs.

---
*Status: Blatt 13 & 12 Complete. Ready for Blatt 11.*