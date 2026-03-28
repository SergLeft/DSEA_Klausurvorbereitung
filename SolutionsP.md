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

## 🧱 Topic 8: The Lego Factory (Matrix Chain Multiplication)

### 🧠 The Theory: The Bottleneck Principle

When you multiply two matrices, $(n \times m)$ and $(m \times p)$, the "cost" is $n \cdot m \cdot p$. The goal is to choose the grouping (parentheses) that creates the smallest intermediate matrices to keep the "bill" low.

#### ✅ Solution Blatt 9, Aufgabe 1: Choosing Groupings

**Scenario**: Matrices $A (10 \times 100), B (100 \times 5), C (5 \times 50)$.

**Decision A: $(A \times B) \times C$**
- Step 1 ($A \times B$): $10 \cdot 100 \cdot 5 = 5,000$ operations. Result: $(10 \times 5)$.
- Step 2 (Result $\times C$): $10 \cdot 5 \cdot 50 = 2,500$ operations.
- **Total: 7,500 operations.**

**Decision B: $A \times (B \times C)$**
- Step 1 ($B \times C$): $100 \cdot 5 \cdot 50 = 25,000$ operations. Result: $(100 \times 50)$.
- Step 2 ($A \times$ Result): $10 \cdot 100 \cdot 50 = 50,000$ operations.
- **Total: 75,000 operations.**

**The Justification**: We pick **Decision A** because it creates a small intermediate "Lego plate" ($(10 \times 5)$) early on. Decision B creates a massive $100 \times 50$ plate that makes the next step 10 times more expensive.

---

## 🍰 Topic 9: The Wedding Cake (Cake Stacking)

### 🧠 The Theory: Sorting for Structure

You want to build the tallest possible tower of cake layers $(w_i, h_i)$. A layer can only go on top if it is strictly thinner ($w_{top} < w_{bottom}$).

#### ✅ Solution Blatt 9, Aufgabe 2: Step-by-Step Stacking

1. **The Decision**: First, sort all layers by **Width** ($w$) from largest to smallest. This ensures we only look "forward" in the list to find layers to put on top.
2. **The Steps**:
   - For each layer $i$, check all layers $j$ that are thinner ($w_j < w_i$).
   - The tallest tower ending with layer $i$ is: $H(i) = h_i + \max(\{H(j) \mid w_j < w_i\} \cup \{0\})$.
3. **The Logic**: This isn't just "guesswork"—by recording the height of the tallest tower for every layer, we only have to solve each sub-tower once.

---
*Status: Blatt 13 down to 9 Complete. Standing by for Blatt 8!* 

## 🌳 Topic 10: AVL Trees (The Gymnasts of Data Structures)

### 🧠 The Theory: Balance is Speed

Imagine a library where all the books are in one long, straight line. To find the last book, you have to walk past every single one. That's $O(n)$—it's slow. If we arrange them in a balanced tree, you only ever need to make a few "Left or Right?" decisions. That's $O(\log n)$. 

- **The Problem:** If you keep adding books to one side, the tree becomes a "stick."
- **The AVL Law:** For every single node, the height of the left and right subtrees can differ by **at most 1**.
- **The Fix (Rotations):** If the height difference becomes **2**, the tree is "wobbly." We perform a rotation (moving a child up to become the parent) to pull the tree back into balance.

### ✅ Solution Trace for 8.1b (1, 0, 5, 4, 3, 2, 6)

1.  **Insert 1**: Root is 1.
2.  **Insert 0**: 0 is left child of 1. Balanced.
3.  **Insert 5**: 5 is right child of 1. Balanced.
4.  **Insert 4**: 4 is left child of 5. Balanced.
5.  **Insert 3**: 3 is left child of 4. Node 5 is now unbalanced (Height Left=2, Right=0). 
    - **Rotation**: **Right Rotation** at 5. 4 becomes parent of 3 and 5.
6.  **Insert 2**: 2 is left child of 3. Node 1 is now unbalanced (Height Left=1, Right=3).
    - **Rotation**: **Left Rotation** at 1. 4 becomes the new root.
7.  **Insert 6**: 6 is right child of 5.

**Final AVL Tree structure**:
- Root: 4
- Left Subtree: 2 (Parent of 1 and 3, with 0 as child of 1)
- Right Subtree: 5 (Parent of 6)

## 🍰 Topic 11: Dynamic Programming (The "Torten-Stapler")

### 🧠 The Theory: Don't Guess, Remember!

Professor Hirnriss wants to know how many ways he can stack $n$ cake pieces using pans of size 1, 2, or 5.

- **ADHD Tip (Analogy):** Think of this like a **Video Game Save Point**. If you know how to get through Level 4, you don't start from Level 1 every time you try Level 5. You just add the last step.
- **The Formula:** To make a cake of size $n$, your last layer was either size 1, 2, or 5.
    - `Total(n) = Total(n-1) + Total(n-2) + Total(n-5)`
- **The "Bottom-Up" Way:** We start with $n=0$ (1 way: don't build a cake!) and $n=1, 2, 3...$ filling out a table until we hit our target $n$.

### ✅ Solution 8.2a (n=5)
Using pans {1, 2, 5}:
- 1+1+1+1+1
- 1+1+1+2 (and permutations)
- 1+2+2 (and permutations)
- 5
(Total ways: 9)

**b) The Algorithm:**
```python

def count_ways(n, V):
    # T[i] = number of ways to build a cake of size i
    T = [0] * (n + 1)
    T[0] = 1 # Base case: 1 way to make a "0-layer" cake (empty)
    
    for i from 1 to n:
        for size in V:
            if i - size >= 0:
                T[i] += T[i - size]
    return T[n]
```
**Complexity**: $O(n \cdot |V|)$. Much faster than trying every combination!

---

## 🎲 Topic 12: Hashing (The Art of Organized Chaos)

## Overview
Hashing is a technique used to map data of arbitrary size to fixed-size values. The output is typically referred to as a hash code, hash value, or hash digest.

## Key Concepts
- **Hash Function**: A function that converts an input into a fixed-size string of bytes. The output is typically a numerical representation of the input data.
- **Collision**: A situation that occurs when two different inputs produce the same hash output.
- **Applications**: Hashing is widely used in data structures (like hash tables), databases, cryptography, and more.


### 🧠 The Theory: The "Perfect" Map
Imagine you have a huge library (the **Universe $U$**) but a very small shelf (the **Hash Table $m$**). You want to store your favorite books so that you can find any book *instantly* ($O(1)$).

- **The Hash Function ($h$):** This is your "Sorting Assistant." It takes a book title (the **Key**) and tells you exactly which spot on the shelf it belongs to.
- **The Nightmare (Collisions):** If your assistant tells two different books to go to the same spot, they crash! This is a collision. In the "Worst Case," every single book is sent to the same spot, and our $O(1)$ dream becomes an $O(n)$ nightmare.
- **The Solution (Universal Hashing):** Since we can't build one perfect function for every possible set of books, we pick a **Random Function** from a special "Class" of functions ($H$). 
    - **The "2-Universal" Guarantee:** A class is 2-universal if, for any two different books, the chance they collide is at most $1/m$. It’s like saying: "My assistant might make a mistake, but they are so consistently random that they'll only mess up as rarely as mathematically possible."

### 🛠 Aufgabe 2: Tanzparty Reloaded (The "Partner" Search)

**The Scenario:** You have a list of people with different heights $l_i$. You need to find pairs that add up to a specific height $h$ (so $l_i + l_j = h$).

**Strategy:**
1.  **The Algorithm:**
    - Create an empty Hash Table $T$.
    - For each height $l$ in our list $L$:
        1.  Check if $target = (h - l)$ is already in $T$.
        2.  If **YES**: Found a pair!
        3.  If **NO**: Put the current height $l$ into $T$.
2.  **Why Hashing?** Because looking up $target$ in the Hash Table takes **expected $O(1)$** time. Doing this for $n$ people gives us an **expected $O(n)$** total time.

### 🔢 Aufgabe 3: 2-Universal Math Trace

We are using the family: $h_a(x) = \sum a_i \cdot x_i \pmod p$.

**✅ Step-by-Step Trace (Part 1):**
- **Given:** $p = 11$, $a = (6, 1, 7, 4)$, $x = (1, 3, 3, 7)$, $y = (4, 7, 1, 1)$.
- **h_a(x):** $(6\cdot1) + (1\cdot3) + (7\cdot3) + (4\cdot7) = 58$. Result: $58 \pmod{11} = 3$.
- **h_a(y):** $(6\cdot4) + (1\cdot7) + (7\cdot1) + (4\cdot1) = 42$. Result: $42 \pmod{11} = 9$.
- **Collision?** No ($3 \neq 9$).

**✅ Step-by-Step Trace (Part 2): Finding the "Collision Constant" $c$**
Set $a_2 = c$. We want $h_{a(c)}(x) \equiv h_{a(c)}(y) \pmod{11}$.
1.  **Equation:** $(37 + 3c) \equiv (35 + c) \pmod{11}$.
2.  **Simplify:** $2c \equiv -2 \equiv 9 \pmod{11}$.
3.  **Solve:** Multiply by modular inverse of 2 ($\pmod{11}$ is 6).
    - $c \equiv 9 \cdot 6 = 54 \equiv 10 \pmod{11}$.
- **Final Answer:** A collision occurs only when **$c = 10$**.

---
*Status: Blatt 13 down to 7 Complete.*
