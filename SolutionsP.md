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
  - Total $S \approx \frac{n}{2} \cdot 2 = n$. Result: **$O(n)$**.

#### ✅ Solution 1b: Step-by-Step Trace

**Initial Array:** `[4, 1, 5, 5, 2, 6, 7, 3, 4, 6, 8, 3]`

1. **$i=5$ (Val 6)**: $6 \ge 3$. OK.
2. **$i=4$ (Val 2)**: Children are 6 and 8. **Swap 2 and 8.**
3. **$i=3$ (Val 5)**: $5 \ge \max(3, 4)$. OK.
4. **$i=2$ (Val 5)**: Children are 6 and 7. **Swap 5 and 7.**
5. **$i=1$ (Val 1)**: Children are 5 and 8. **Swap 1 and 8.** Trickle 1 down: Children are 6 and 2. **Swap 1 and 6.**
6. **$i=0$ (Val 4)**: Children are 8 and 7. **Swap 4 and 8.** Trickle 4 down: Children are 5 and 6. **Swap 4 and 6.**

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

## 🗺️ Topic 5: Shortest Paths (Navigation Suite)

### 🧠 The Theory: Navigation Strategies

Shortest path algorithms are our "Navigation Suite." They help us find the fastest way from point A to point B.

- **Dijkstra: "The Greedy GPS"**
  - **Scenario**: You want the fastest route from Home to the Airport.
  - **Strategy**: It always picks the closest place it hasn't visited yet and fixes it. It's **Greedy** because it assumes that once a place is fixed, it has found the absolute best way there.
  - **The "Negative Weight" Trap**: Dijkstra only works if all roads have $\ge 0$ travel time. If a road has "negative time" (a warp hole), Dijkstra misses it because it never looks back once a node is "fixed."

- **Floyd-Warshall: "The Class Representative"**
  - **Scenario**: You want a table that tells you the shortest path between **every single pair** of points in the city.
  - **Strategy**: It picks one point at a time ($K$) and asks: "For every pair $(i, j)$, is it faster to go directly or **through $K$**?"
  - **The "In-Place" Trick**: We can update the table directly because the distance to/from point $K$ doesn't change when $K$ is the intermediate point. This saves massive memory.

### ✅ Solution Blatt 11, Aufgabe 1: Floyd-Warshall Table Reduction

**Question**: Explain why we can update the matrix $D^{(k)}$ in place (using only one matrix).
**Explanation**:
The update rule is $d_{ij} = \min(d_{ij}, d_{ik} + d_{kj})$. To do this "in place," we must ensure that the values we use ($d_{ik}$ and $d_{kj}$) aren't changed by the update itself in the current round $k$.
1. A shortest path from $i$ to $k$ that is allowed to use $k$ as an intermediate point cannot be shorter than a path that *doesn't* use $k$ as an intermediate.
2. Therefore, $d_{ik}^{(k)} = d_{ik}^{(k-1)}$ and $d_{kj}^{(k)} = d_{kj}^{(k-1)}$.
3. Since these "helper" values stay the same throughout round $k$, we can safely overwrite the matrix.

### ✅ Solution Blatt 11, Aufgabe 2: Dijkstra Trace

**Graph Edges**: (s-a: 1), (s-c: 4), (a-b: 2), (a-c: 2), (b-c: 1), (b-t: 6), (c-t: 3).

1. **Step 0**: `dist[s]=0`, others $\infty$. Priority Queue: `{(s,0)}`.
2. **Step 1 (Fix s)**: Update neighbors: `dist[a]=1`, `dist[c]=4`. PQ: `{(a,1), (c,4)}`.
3. **Step 2 (Fix a)**: Update neighbors: `dist[b] = 3`, `dist[c] = \min(4, 1+2)=3`. PQ: `{(b,3), (c,3)}`.
4. **Step 3 (Fix b)**: Update neighbors: `dist[c] = \min(3, 3+1)=3`, `dist[t] = 3+6=9`. PQ: `{(c,3), (t,9)}`.
5. **Step 4 (Fix c)**: Update neighbors: `dist[t] = \min(9, 3+3)=6`. PQ: `{(t,6)}`.
6. **Step 5 (Fix t)**: Shortest path from $s$ to $t$ is **6**. (Path: $s \to a \to c \to t$).

---

## 🎨 Topic 6: The Master of Efficiency (Huffman Coding)

### 🧠 The Theory: The "Frequent Flyer" Principle

Give the items you use **most often** the **smallest** bit-codes (boxes). 

- **Prefix-Free Property**: No code can start with another code. This ensures a unique, clear "unboxing" (decoding) process.

#### ✅ Solution Blatt 10, Aufgabe 1: Huffman Coding "ESGIBTFREIBIER"

**1. Frequencies:** E:3, I:3, R:2, B:2, S:1, G:1, T:1, F:1.
**2. The Codes (Trace):**
- **E**: 00, **I**: 01
- **R**: 100, **B**: 101
- **S**: 1100, **G**: 1101, **T**: 1110, **F**: 1111

**3. Total Bits:**
- E, I (6 total chars) x 2 bits = 12
- R, B (4 total chars) x 3 bits = 12
- S, G, T, F (4 total chars) x 4 bits = 16
- **Total: 40 Bits.** (Compared to 112 bits in 8-bit ASCII!)

---

## ⛓️ Topic 7: The "Strict Guest List" (Degrees & Components)

### 🧠 The Theory: Graph Formations

If every person in a room can hold at most **two hands** ($deg(v) \le 2$), there are only three possible "party formations":
1. **The Loner**: 0 hands. (Isolated Node).
2. **The Line**: 1 hand for ends, 2 for the middle. (Simple Path).
3. **The Circle**: 2 hands for everyone. (Simple Cycle).

#### ✅ Solution Blatt 10, Aufgabe 2: Induction Proof

- **Base Case**: A single node is a "Loner."
- **Induction**: Adding a node to an existing group either starts a new group, extends a "Line," or connects the ends of a "Line" to form a "Circle." No other connections are possible without someone holding a 3rd hand!

---
*Status: Blatt 13, 12, 11, & 10 Complete. Standing by for Blatt 9!*