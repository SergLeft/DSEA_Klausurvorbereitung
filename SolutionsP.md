# Solutions and Theory for Präsenzblätter

## 🏗 Topic 1: Heaps (The Priority Engine)

### 🧠 The Theory: Why and What?

Imagine an **Emergency Room (ER)**. Patients aren't treated in the order they arrive; they are treated by **Priority**.

- **The Problem**: A sorted list is slow for adding patients $(O(n))$. An unsorted list is slow for finding the most urgent one ($O(n)$).
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

## 📉 Topic 13: Lower Bounds and Randomized Selection

### 🧠 The Theory: The Wall of Information

In algorithms, we often look for the "fastest" way. But sometimes, mathematics tells us there is a **physical limit**—a wall you cannot pass.

- **Information Theory Lower Bound:** 
  Imagine I'm thinking of a number between 1 and $n$. You ask "Yes/No" questions. Each question cuts the possibilities in half. 
  - To find one specific item out of $n$, you need at least $\log_2(n)$ questions.
  - This is why **Binary Search** is $O(\log n)$ and why you **cannot** find an element in a sorted list faster than that using only comparisons. You simply haven't gathered enough "information" yet!
  - For **Sorting**, there are $n!$ possible ways to order a list. To find the *one* correct order, you need $\log_2(n!) \approx n \log n$ comparisons. That's the wall!

- **Randomized Selection (QuickSelect):**
  This is QuickSort's smarter sibling. Instead of sorting the *whole* array, you only care about one specific rank (e.g., the Median).
  - **The Strategy:** Pick a "Pivot." Partition the array. If the item you want is in the left half, **ignore the right half entirely**.
  - **The "Median of Three" Trick:** Picking a random pivot can be risky (what if you pick the smallest item?). If you pick 3 random items and use their median as the pivot, you are much more likely to land "near the middle," making the algorithm faster and more stable.

### ✅ Solution 6.1: The Lower Bound for Search

**Question:** Show that finding an element in a sorted list takes at least $\Omega(\log n)$ comparisons.

**The Decision Tree Proof:**
1.  Any comparison-based algorithm can be viewed as a **Decision Tree**.
2.  Each internal node is a comparison (e.g., "Is $x < A[i]$?"). Each leaf is a possible outcome (the index where $x$ is found, or "Not Found").
3.  There are $n+1$ possible outcomes (it could be at any of the $n$ positions, or not there at all).
4.  A binary tree with $L$ leaves must have a height of at least $\lceil \log_2 L \rceil$.
5.  Therefore, the height (worst-case number of comparisons) is at least $\log_2(n+1)$, which is $\Omega(\log n)$.

### ✅ Solution 6.2: Randomized Partition (Median of 3)

**Scenario:** We pick 3 distinct elements and use their median as the pivot.

**a) Probability $p_i$:**
The element with rank $i$ becomes the pivot if it is the median of the 3 chosen elements.
- This means one chosen element must be smaller (from the $i-1$ options) and one must be larger (from the $n-i$ options).
- **Formula:** $p_i = \frac{(i-1) \cdot (n-i)}{\binom{n}{3}}$
- *Note: We multiply by 1 because the rank $i$ element is fixed as the middle choice.*

**b) Median Improvement (Limit $n \to \infty$):**
- **Old way (1 random pivot):** Chance of hitting the exact median is $1/n$.
- **New way (Median of 3):** 
  - Let $i = n/2$ (the median).
  - $p_{median} \approx \frac{(n/2) \cdot (n/2)}{n^3/6} = \frac{n^2/4}{n^3/6} = \frac{1.5}{n}$.
- **Ratio:** $\frac{1.5/n}{1/n} = 1.5$.
- **Conclusion:** You are **50% more likely** to hit the median using the "Median of 3" strategy compared to picking just one random element. This drastically reduces the chance of the "Worst Case" $O(n^2)$ behavior!

---

## 📘 Topic 14: Präsenzblatt 5 — Full Tutoring Session (ADHD-Friendly, From Zero)

### 🧠 Aufgabe 1: Polynomial Multiplication via DFT

#### 1) What is this asking?
You are given two polynomials:
- $A(x)=1+2x$
- $B(x)=3-x$

You need to compute:
- $C(x)=A(x)\cdot B(x)$

but using the **DFT** method (not only direct multiplication).

- **DFT** = **Discrete Fourier Transform**

---

#### 2) Core ideas from zero

##### What is a polynomial?
A polynomial is an expression like:
- $a_0 + a_1x + a_2x^2 + \dots$

The numbers $a_0,a_1,a_2,\dots$ are called **coefficients**.

Example:
- $1+2x = 1\cdot x^0 + 2\cdot x^1$
- Coefficient vector: $(1,2)$

##### Why use DFT for multiplication?
Multiplying in coefficient form is a convolution-like operation.  
DFT gives a powerful shortcut:

1. Convert coefficients to values at special points.
2. Multiply those values component-wise.
3. Convert back to coefficients.

Think of it as changing to a coordinate system where multiplication is easier.

##### Roots of unity (special evaluation points)
For length $n$, a primitive root is:
- $\omega_n = e^{2\pi i/n}$

Here length is $4$, so:
- $\omega_4 = e^{2\pi i/4}=i$
- and $i^2=-1$

Thus the four points are:
- $\omega_4^0=1$
- $\omega_4^1=i$
- $\omega_4^2=-1$
- $\omega_4^3=-i$

##### Why pad to length 4?
Product degree can go up to degree $2$.  
The task instructs length 4, so we use zero-padding:
- $a'=(1,2,0,0)$
- $b'=(3,-1,0,0)$

---

#### 3) Step-by-step solution

We use:
- $A(x)=1+2x$
- $B(x)=3-x$
- $\omega_4=i$

##### Step A: Compute $DFT(a')$
Evaluate $A$ at $1,i,-1,-i$:

- $A(1)=1+2=3$
- $A(i)=1+2i$
- $A(-1)=1-2=-1$
- $A(-i)=1-2i$

So:
- $DFT(a')=(3,\;1+2i,\;-1,\;1-2i)$

##### Step B: Compute $DFT(b')$
Evaluate $B$ at $1,i,-1,-i$:

- $B(1)=3-1=2$
- $B(i)=3-i$
- $B(-1)=3-(-1)=4$
- $B(-i)=3-(-i)=3+i$

So:
- $DFT(b')=(2,\;3-i,\;4,\;3+i)$

##### Step C: Component-wise multiplication
Let $\widehat c_k = \widehat a_k\widehat b_k$:

- $k=0:\;3\cdot2=6$
- $k=1:\;(1+2i)(3-i)=5+5i$
- $k=2:\;(-1)\cdot4=-4$
- $k=3:\;(1-2i)(3+i)=5-5i$

Hence:
- $DFT(c')=(6,\;5+5i,\;-4,\;5-5i)$

##### Step D: Inverse DFT
Applying inverse DFT gives:
- $c'=(3,5,-2,0)$

Therefore:
- $\boxed{C(x)=3+5x-2x^2}$

##### Quick sanity check
Direct multiplication:
- $(1+2x)(3-x)=3-x+6x-2x^2=3+5x-2x^2$

Matches exactly ✅

---

#### 4) Common exam mistakes
1. Forgetting zero-padding.
2. Using wrong root of unity.
3. Sign errors with $i^2=-1$.
4. Forgetting normalization in inverse DFT.
5. Skipping sanity check.

---

#### 5) Micro-summary
- DFT turns polynomial multiplication into easy component-wise multiplication.
- For length 4, evaluate at $1,i,-1,-i$.
- Final answer:
  - $\boxed{C(x)=3+5x-2x^2}$

---

### 🎲 Aufgabe 2: Probability with Two Dice

#### 1) What is this asking?
Alice throws two fair six-sided dice at once:
- one red,
- one green.

Let $S$ be the sum.

You need:
1. The sample space.
2. Whether events $S\ge 6$ and $S\le 3$ are disjoint and independent.
3. For $n$ double-throws, the expected number with $S\ge 6$.

---

#### 2) Core ideas from zero

##### Sample space
Because dice are distinguishable (red vs green), outcomes are ordered pairs:
- $\Omega=\{(r,g)\mid r,g\in\{1,2,3,4,5,6\}\}$

So:
- $|\Omega|=36$
- each outcome has probability $1/36$

##### Event
An event is a subset of $\Omega$ (for example all outcomes with sum at least 6).

##### Disjoint events
$A,B$ are disjoint if:
- $A\cap B=\varnothing$
(meaning they cannot happen together).

##### Independent events
$A,B$ are independent if:
- $P(A\cap B)=P(A)P(B)$
(meaning one does not influence the other).

##### Expected count in repeated trials
If success probability per trial is $p$, then for $n$ trials:
- $E[X]=np$

---

#### 3) Step-by-step solution

Let:
- $A=\{S\ge 6\}$
- $B=\{S\le 3\}$

##### Part 1: Sample space
- $\Omega=\{(r,g)\mid r,g\in\{1,\dots,6\}\}$
- $|\Omega|=36$

##### Part 2a: Disjoint?
A sum cannot be both $\ge 6$ and $\le 3$.  
So:
- $A\cap B=\varnothing$

Yes, disjoint ✅

##### Part 2b: Independent?
Need to check:
- $P(A\cap B)=P(A)P(B)$

Left side:
- $P(A\cap B)=0$

Now compute right side.

For $B$ (sum $\le 3$):
- sum 2: 1 outcome
- sum 3: 2 outcomes
- total 3 outcomes

So:
- $P(B)=3/36=1/12$

For $A$ (sum $\ge 6$), use complement:
- sums 2,3,4,5 have counts $1+2+3+4=10$
- so $P(A)=1-10/36=26/36=13/18$

Thus:
- $P(A)P(B)=\frac{13}{18}\cdot\frac{1}{12}=\frac{13}{216}>0$

Since $0\ne 13/216$, they are **not independent** ❌

##### Part 3: Expected number in $n$ double-throws
Success = event $S\ge 6$, with probability:
- $p=13/18$

Let $X$ = number of successes in $n$ throws. Then:
- $E[X]=np=n\cdot\frac{13}{18}$

Final:
- $\boxed{E[X]=\frac{13n}{18}}$

---

#### 4) Common exam mistakes
1. Treating dice as indistinguishable.
2. Confusing disjointness and independence.
3. Wrong counting of sum outcomes.
4. Not using complement for quicker counting.
5. Forgetting $E[X]=np$.

---

#### 5) Micro-summary
- 36 ordered outcomes.
- $S\ge 6$ and $S\le 3$: disjoint, but not independent.
- $P(S\ge 6)=13/18$.
- Expected count in $n$ trials:
  - $\boxed{E[X]=13n/18}$

---

---

## 📘 Topic 15: Präsenzblatt 4 — Full Tutoring Session (ADHD-Friendly, From Zero)

### 🧠 Aufgabe 1a: Binary Search Recurrence and Runtime

#### 1) What is this asking?
You need:
1. The recurrence equation for binary search.
2. Its asymptotic runtime.
3. The Master Theorem parameters $a,b,\alpha$.

---

#### 2) Core ideas from zero

##### What is a recurrence?
A recurrence expresses runtime $T(n)$ in terms of smaller input sizes.

General form:
- $T(n)=aT(n/b)+f(n)$

where:
- $a$ = number of recursive subproblems,
- $n/b$ = size of each subproblem,
- $f(n)$ = extra work outside recursion.

##### How binary search works
Binary search assumes a sorted list:
1. Compare target with middle element.
2. Keep only one half.
3. Repeat.

So each step does:
- one recursive call on size $n/2$,
- constant extra work.

---

#### 3) Build the recurrence
For $n\ge 2$:
- $T(n)=T(n/2)+c$, with $c>0$ constant and $T(1)=\Theta(1)$.

Equivalent form:
- $T(n)=T(n/2)+\Theta(1)$.

---

#### 4) Runtime
Each step halves the problem size, so number of steps is about $\log_2 n$.  
Each level costs constant time.

Therefore:
- $\boxed{T(n)=\Theta(\log n)}$

---

#### 5) Master Theorem parameters
Match:
- $T(n)=aT(n/b)+\Theta(n^\alpha)$

Here:
- $a=1$
- $b=2$
- $\Theta(1)=\Theta(n^0)$, so $\alpha=0$

Also:
- $\log_b a=\log_2 1=0$

Equal case $\alpha=\log_b a$, so:
- $T(n)=\Theta(n^\alpha\log n)=\Theta(\log n)$

---

#### 6) Micro-summary
- Recurrence: $\boxed{T(n)=T(n/2)+\Theta(1)}$
- Runtime: $\boxed{\Theta(\log n)}$
- Parameters: $\boxed{a=1,\;b=2,\;\alpha=0}$

---

### 🧮 Aufgabe 1b: Karatsuba Multiplication for $4711\cdot1337$

#### 1) What is this asking?
Compute $4711\cdot1337$ using the **Karatsuba algorithm**, with recursion stop at 2-digit numbers.

---

#### 2) Core Karatsuba concept (from zero)
Karatsuba is a faster divide-and-conquer multiplication method.

Naive split of two $n$-digit numbers uses 4 half-size multiplications.  
Karatsuba reduces this to 3 half-size multiplications plus additions/subtractions.

That improves asymptotic complexity:
- $T(n)=3T(n/2)+\Theta(n)$
- $\Rightarrow \Theta(n^{\log_2 3})\approx\Theta(n^{1.585})$

---

#### 3) Karatsuba formula
Write:
- $x=a\cdot 10^m+b$
- $y=c\cdot 10^m+d$

Then:
- $xy = ac\cdot 10^{2m} + \big((a+b)(c+d)-ac-bd\big)\cdot 10^m + bd$

So only 3 multiplications are needed:
- $ac$
- $bd$
- $(a+b)(c+d)$

---

#### 4) Apply to $4711$ and $1337$
Split after 2 digits ($m=2$):
- $4711=47\cdot100+11$ so $a=47,b=11$
- $1337=13\cdot100+37$ so $c=13,d=37$

Compute:
- $ac=47\cdot13=611$
- $bd=11\cdot37=407$
- $(a+b)(c+d)=58\cdot50=2900$

Middle term:
- $(a+b)(c+d)-ac-bd=2900-611-407=1882$

Recombine:
- $xy = 611\cdot10000 + 1882\cdot100 + 407$
- $=6{,}110{,}000 + 188{,}200 + 407$
- $=6{,}298{,}607$

Final:
- $\boxed{4711\cdot1337=6{,}298{,}607}$

---

#### 5) Common exam mistakes
1. Splitting with wrong base.
2. Forgetting middle-term formula.
3. Place-value errors in recombination.
4. Arithmetic slips in subtraction.

---

#### 6) Micro-summary
Karatsuba workflow:
1. Split numbers.
2. Compute $ac$, $bd$, $(a+b)(c+d)$.
3. Middle term = third minus first minus second.
4. Recombine with powers of $10^m$.

Result:
- $\boxed{6{,}298{,}607}$

---

### 📐 Aufgabe 2: Master Theorem Recurrences

Given:
- $T(1)=1$
- $n$ is a power of 2
- $c>0$
- for $n\ge2$

General form:
- $T(n)=aT(n/b)+\Theta(n^\alpha)$

---

#### a) $T(n)=4T(n/2)+cn^2$

Parameters:
- $a=4,\;b=2,\;\alpha=2$
- $\log_b a=\log_2 4=2$

Equal case $\alpha=\log_b a$:
- $T(n)=\Theta(n^\alpha\log n)=\Theta(n^2\log n)$

Final:
- $\boxed{T(n)=\Theta(n^2\log n)}$

---

#### b) $T(n)=3T(n/2)+cn$

Parameters:
- $a=3,\;b=2,\;\alpha=1$
- $\log_2 3\approx1.585$

Here $n^\alpha$ grows slower than $n^{\log_2 3}$, so recursion dominates:
- $T(n)=\Theta(n^{\log_2 3})$

Final:
- $\boxed{T(n)=\Theta(n^{\log_2 3})}$

---

#### c) $T(n)=2T(n/2)+c\log_2 n$

This is not a pure $\Theta(n^\alpha)$ polynomial term, so use recursion-tree intuition:

- At level $i$: $2^i$ subproblems, each of size $n/2^i$
- Per-node non-recursive cost: $\log(n/2^i)=\log n - i$
- Level cost: $2^i(\log n-i)$
- Summing over levels gives $\Theta(n)$ total non-leaf work
- Leaves add $\Theta(n)$

So:
- $\boxed{T(n)=\Theta(n)}$

---

### ✅ Final compact answers (exam-style)

1. **Aufgabe 1a**  
   - $T(n)=T(n/2)+\Theta(1)$  
   - $T(n)=\Theta(\log n)$  
   - $a=1,\;b=2,\;\alpha=0$

2. **Aufgabe 1b**  
   - $4711\cdot1337=6{,}298{,}607$

3. **Aufgabe 2a**  
   - $T(n)=\Theta(n^2\log n)$

4. **Aufgabe 2b**  
   - $T(n)=\Theta(n^{\log_2 3})$

5. **Aufgabe 2c**  
   - $T(n)=\Theta(n)$

---
---

## 📘 Topic 16: Präsenzblatt 3 — Full Tutoring Session (ADHD-Friendly, From Zero)

### 🧠 Aufgabe 1: False Induction (Why the “Merge Sort is O(n)” proof is wrong)

#### 1) What is this asking?
You are given a proof attempt that claims Merge Sort runs in $O(n)$.  
You must identify exactly where the proof fails.

---

#### 2) Abbreviations and concepts (from zero)

- **IA** = *Induktionsanfang* (base case)
- **IV** = *Induktionsvoraussetzung* (induction hypothesis)
- **IS** = *Induktionsschritt* (induction step)
- **$O(\cdot)$** = asymptotic upper bound
- **$\Omega(\cdot)$** = asymptotic lower bound
- **$\Theta(\cdot)$** = asymptotically tight bound (both upper and lower)

Merge Sort recurrence:
- $T(n)\le 2T(\lceil n/2\rceil)+cn$ with $c>0$

Known correct result:
- $T(n)=\Theta(n\log n)$

So if a proof claims $O(n)$, there must be a logic mistake.

---

#### 3) What the faulty proof does
In the induction step it says:
- $T(n)\le 2T(\lceil n/2\rceil)+cn$
- by IV, $T(\lceil n/2\rceil)\in O(n)$
- therefore first term is in $O(n)$, second term in $O(n)$
- so sum is in $O(n)$

This sounds plausible, but it is not rigorous enough for this induction claim.

---

#### 4) Exact logical mistake
The proof uses Big-O too loosely and does not track constants in a closed induction inequality.

A valid induction for linear bound must look like:
- Assume for all $k<n$: $T(k)\le Ck$ (fixed constant $C$)
- Then show: $T(n)\le Cn$

Now plug into recurrence:
- $T(n)\le 2C\lceil n/2\rceil + cn \approx Cn + cn = (C+c)n$

To conclude $T(n)\le Cn$, we would need:
- $(C+c)n\le Cn \Rightarrow c\le 0$

But $c>0$, contradiction.  
So the linear claim cannot be maintained.

So the exact issue is:
- The induction step hides growth in the extra $+cn$ term and does not preserve a fixed constant bound.

---

#### 5) Analogy (how theory binds to the solution)
Imagine monthly expenses:
- You claim: “I always stay under 1000€.”
- But each month your old plan adds a fixed unavoidable fee.
- If you keep saying “still around 1000€” without updating exact numbers, your claim is mathematically broken.

That is what happened in the bad proof:
- The $+cn$ is the unavoidable fee.
- Big-O handwaving hid that the budget (constant $C$) no longer closes.

---

#### 6) Correct conclusion
The induction argument is invalid.  
Correct asymptotic runtime remains:
- $\boxed{T(n)=\Theta(n\log n)}$

---

#### 7) Micro-summary
- Error is in IS (induction step).
- Big-O notation was used non-rigorously in place of a fixed inequality.
- Extra linear merge term prevents proving linear total runtime.

---

### 🧠 Aufgabe 2: Every comparison-based sorting algorithm can be made stable

#### 1) What is this asking?
Show that any comparison-based sorting algorithm can be transformed into a stable one.

---

#### 2) Core concepts (from zero)

##### Stable sorting
A sorting algorithm is **stable** if equal keys keep their original relative order.

Example:
- Input: $(5,A), (3,B), (5,C)$
- Sorted by key: $(3,B), (5,A), (5,C)$  
Stable means $A$ stays before $C$ because it appeared earlier.

##### Comparison-based sorting
The algorithm only learns via comparisons between elements (e.g., $<$, $>$, $=$), not via bucket/radix/counting tricks.

---

#### 3) Construction that makes any such algorithm stable
Replace each element $x_i$ by:
- $(x_i, i)$  
where $i$ is original position in input.

Use comparator:
- $(x_i,i) < (x_j,j)$ iff
  - $x_i < x_j$, or
  - $x_i = x_j$ and $i<j$

So ties are broken by original index.

---

#### 4) Why this works (proof idea)
If two elements are equal in value and one appeared earlier, index tie-break guarantees earlier one is “smaller” in comparison, hence must be placed first.  
Therefore equal values preserve input order by construction.

So transformed algorithm is stable.

---

#### 5) Complexity impact
You only add constant-time tie-break logic in comparisons.  
Asymptotic number of comparisons does not change class.  
So runtime class stays the same asymptotically.

---

#### 6) Analogy (how theory binds to the solution)
Think of exam papers:
- Primary sorting key = score.
- Tie-breaker = submission timestamp (or queue number).
Then equal-score papers keep arrival order automatically.  
That is exactly stability via secondary key.

---

#### 7) Micro-summary
To stabilize any comparison-based sorter:
1. Attach original index.
2. Compare by (key, index).
3. Equal keys now remain in input order.

Final statement:
- $\boxed{\text{Every comparison-based sorting algorithm can be made stable.}}$

---

### ✅ Final compact answers (exam-style)

1. **Aufgabe 1**  
   The proof fails in the induction step because Big-O is used informally instead of maintaining a fixed inequality $T(n)\le Cn$. The additive $+cn$ term breaks closure of a linear bound. Correct runtime is:
   - $\boxed{T(n)=\Theta(n\log n)}$

2. **Aufgabe 2**  
   Augment each element with original position and compare lexicographically by (key, index). This enforces stable order for equal keys, so any comparison-based sorting algorithm can be made stable.

---

---

## 📘 Topic 17: Präsenzblatt 2 — Full Tutoring Session (ADHD-Friendly, From Zero)

### 🧠 Aufgabe 1: Landau Symbols and Asymptotic Growth Ordering

We are given these functions:
- $f_1(n)=\pi n$
- $f_2(n)=10^{1/n}$
- $f_3(n)=2^n$
- $f_4(n)=n\ln n$
- $f_5(n)=\log(n\log n)$
- $f_6(n)=\sqrt{n^3}=n^{3/2}$

We must order them by asymptotic growth using:
- $f\preceq g \iff f\in O(g)$

---

#### 1) Concepts and abbreviations

- **Landau symbols**: $O(\cdot),\Omega(\cdot),\Theta(\cdot)$
- $f\in O(g)$: $f$ grows no faster than $g$ (up to constant factors eventually)
- $\preceq$: “grows no faster than”

---

#### 2) Decision strategy (why this order of attack?)
When multiple mixed functions appear, use this decision ladder:

1. Constants / limit-to-constant terms  
2. Logarithmic terms  
3. Polynomial terms  
4. Polynomial $\times$ logarithm terms  
5. Exponential terms

This prevents confusion and gives a structured way to compare.

---

#### 3) Step-by-step decisions with justifications

##### Step A: Analyze $f_2(n)=10^{1/n}$
Decision: compute the limit as $n\to\infty$:
- $10^{1/n}\to 10^0=1$

So:
- $f_2\in\Theta(1)$ (constant class).

**Analogy:** this is like a startup effect that fades and settles to a fixed baseline.

---

##### Step B: Analyze $f_5(n)=\log(n\log n)$
Use log identity:
- $\log(n\log n)=\log n+\log\log n$

Dominant term is $\log n$, so:
- $f_5\in\Theta(\log n)$.

This is larger than constant but smaller than any positive polynomial power.

---

##### Step C: Analyze $f_1(n)=\pi n$
$\pi$ is just a constant factor, so:
- $f_1\in\Theta(n)$.

---

##### Step D: Analyze $f_6(n)=n^{3/2}$
This is a polynomial of degree $1.5$, so it grows faster than linear $n$.

---

##### Step E: Compare $f_4(n)=n\ln n$ with $f_6(n)=n^{3/2}$
Decision: use ratio test:
- $\dfrac{n\ln n}{n^{3/2}}=\dfrac{\ln n}{\sqrt n}\to 0$

Hence:
- $n\ln n=o(n^{3/2})$

So $n\ln n$ grows slower than $n^{3/2}$.

---

##### Step F: Analyze $f_3(n)=2^n$
Exponential growth dominates all polynomial/logarithmic growth classes, so this is largest.

---

#### 4) Final ordering
From smallest to largest:
- $\boxed{f_2 \prec f_5 \prec f_1 \prec f_4 \prec f_6 \prec f_3}$

Expanded:
- $\boxed{10^{1/n} \prec \log(n\log n) \prec \pi n \prec n\ln n \prec n^{3/2} \prec 2^n}$

---

#### 5) Micro-summary
Use growth ladder:
- constant < log < linear < linear·log < higher polynomial < exponential.

---

### 🧠 Aufgabe 2: Pair Matching in a Sorted List (Partnersuche)

Given:
- sorted list $L=(l_1,\dots,l_n)$ of pairwise distinct heights
- target sum $h$
- count how many pairs satisfy $l_i+l_j=h$

---

#### 1) Core idea from zero
Since the list is already sorted, we can avoid $O(n^2)$ brute force by using two pointers:
- one from left (small values),
- one from right (large values).

---

#### 2) Decision-making: why two pointers?
We choose two pointers because sorted order lets us eliminate impossible candidates safely in each step:
- if sum too small, only increasing the smaller value can help,
- if sum too large, only decreasing the larger value can help.

This creates a linear-time strategy.

---

#### 3) Algorithm (two-pointer method)

Initialize:
- $i=1$ (left pointer),
- $j=n$ (right pointer),
- `count = 0`.

While $i<j$:
1. $s=l_i+l_j$
2. If $s=h$: count++, $i++$, $j--$
3. If $s<h$: $i++$
4. If $s>h$: $j--$

Return `count`.

---

#### 4) Why each move is justified

##### Case 1: $s<h$ → move $i$
Current left value is too small even with the largest available partner.  
Any partner left of $j$ is smaller, so it cannot work either.  
Therefore discarding $l_i$ is safe.

##### Case 2: $s>h$ → move $j$
Current right value is too large even with the smallest available partner.  
Any partner right of $i$ is larger, so it cannot work either.  
Therefore discarding $l_j$ is safe.

##### Case 3: $s=h$ → count and move both
We found a valid pair.  
With distinct values and pairing interpretation, we lock this pair and continue inward.

---

#### 5) Analogy (binds theory to solution)
Think of a seesaw with target balance:
- left child = lightest available
- right child = heaviest available

If too light, replace the lighter side with a heavier one (move left pointer right).  
If too heavy, replace the heavier side with a lighter one (move right pointer left).  
If exactly balanced, record the match and move both inward.

Because the list is sorted, discarded options can never become valid later.

---

#### 6) Correctness argument (exam-ready)

Loop invariant:
At each loop start $(i,j)$, all pairs outside the interval $[i,j]$ are already correctly handled, and discarded elements cannot form valid pairs with remaining ones.

- Initialization: true before first iteration.
- Maintenance: pointer moves discard only impossible candidates (proved above).
- Termination: when $i\ge j$, no candidate pair remains.

Hence algorithm counts all valid pairs correctly.

---

#### 7) Runtime and space
Each iteration moves at least one pointer inward.  
Each pointer moves at most $n$ steps total.

So:
- Runtime: $\boxed{O(n)}$
- Extra space: $\boxed{O(1)}$

---

### ✅ Final compact answers (exam-style)

1. **Aufgabe 1**
   - $\boxed{10^{1/n} \prec \log(n\log n) \prec \pi n \prec n\ln n \prec n^{3/2} \prec 2^n}$

2. **Aufgabe 2**
   - Use two-pointer scan on sorted list.
   - Correct by monotonic elimination and loop invariant.
   - Complexity: $\boxed{O(n)}$ time, $\boxed{O(1)}$ space.

---
---

## 📘 Topic 18: Präsenzblatt 1 — Full Tutoring Session (ADHD-Friendly, From Zero)

### 🧠 Aufgabe 1: Stable Matching, Non-Termination, and a Stable Solution

We have 3 persons $p_1,p_2,p_3$ and 3 jobs $a_1,a_2,a_3$.

Given preferences (interpreting “$<$” as “is preferred over”):
- $p_1: a_3 \succ a_1 \succ a_2$
- $p_2: a_3 \succ a_2 \succ a_1$
- $p_3: a_3 \succ a_2 \succ a_1$
- $a_1: p_2 \succ p_3 \succ p_1$
- $a_2: p_2 \succ p_1 \succ p_3$
- $a_3: p_2 \succ p_1 \succ p_3$

---

#### 1) Concepts and abbreviations

- **Stable matching**: a matching with no blocking pair.
- **Blocking pair / unstable pair** $(p_i,a_j)$: person $p_i$ and job $a_j$ both prefer each other over their current partners.
- **Termination**: algorithm eventually stops.
- **Naive swapping algorithm**: repeatedly choose an unstable pair and swap partners accordingly.

---

#### 2) (a) Why naive algorithm need not terminate

Start matching:
- $Z_0=\{(p_1,a_1),(p_2,a_2),(p_3,a_3)\}$

Key theoretical point:
- The naive algorithm does not prescribe a unique unstable pair choice.
- Different choices can lead to different trajectories.
- Some trajectories can revisit previous matchings (cycle), so termination is not guaranteed.

So the correct conclusion is:
- $\boxed{\text{The naive algorithm is not guaranteed to terminate.}}$

---

#### 3) Decision-making to find a stable matching (part b)

**Decision 1:** Who should get $a_3$?  
All persons like $a_3$ highly, and $a_3$ prefers $p_2$ most.  
If $p_2$ does not get $a_3$, pair $(p_2,a_3)$ tends to block.

So we set:
- $(p_2,a_3)$

Remaining persons/jobs:
- persons: $p_1,p_3$
- jobs: $a_1,a_2$

Now compare remaining preferences:
- $p_1: a_1 \succ a_2$
- $p_3: a_2 \succ a_1$
- $a_1: p_3 \succ p_1$
- $a_2: p_1 \succ p_3$

Choose:
- $(p_1,a_2)$ and $(p_3,a_1)$

So candidate:
- $Z^*=\{(p_2,a_3),(p_1,a_2),(p_3,a_1)\}$

Check potential blocks:
- $(p_1,a_1)$: $p_1$ prefers $a_1$, but $a_1$ prefers $p_3$ over $p_1$ → no block.
- $(p_3,a_2)$: $p_3$ prefers $a_2$, but $a_2$ prefers $p_1$ over $p_3$ → no block.
No blocking pair remains.

Therefore:
- $\boxed{Z^*=\{(p_2,a_3),(p_1,a_2),(p_3,a_1)\}}$ is stable.

Can naive algorithm find $Z^*$ from $Z_0$?
- Yes, for some unstable-pair choices.
- But not guaranteed because it may also cycle.

---

#### 4) Analogy (theory ↔ exercise)
Imagine a dance party:
- People keep switching partners whenever two people mutually prefer each other.
- Without strict rules, the same partner configurations can reappear (loop).
- A stable matching is a “no-drama state” where no two people would rather switch.

---

### 🧠 Aufgabe 2: Logarithm Identities and Simplifications

---

#### 1) (a) Show change-of-base formula
Prove:
- $\log_a(x)=\dfrac{\log_b(x)}{\log_b(a)}$

Proof:
Let $y=\log_a(x)$, so $a^y=x$.  
Take $\log_b$:
- $\log_b(a^y)=\log_b(x)$
- $y\log_b(a)=\log_b(x)$
- $y=\dfrac{\log_b(x)}{\log_b(a)}$

Since $y=\log_a(x)$, done.

---

#### 2) (b) Show power rule
- $\log_a(x^y)=y\log_a(x)$

This is the standard logarithm power identity.

---

#### 3) (c) Simplify $4^{\log_2(n)}$
- $4=2^2$
- $(2^2)^{\log_2 n}=2^{2\log_2 n}=(2^{\log_2 n})^2=n^2$

So:
- $\boxed{4^{\log_2 n}=n^2}$

---

#### 4) (d) Simplify $\dfrac{\ln(n^{\ln n})}{(\ln n)^2}$
- $\ln(n^{\ln n})=(\ln n)(\ln n)=(\ln n)^2$

Hence:
- $\dfrac{(\ln n)^2}{(\ln n)^2}=1$

So:
- $\boxed{1}$

---

#### 5) (e) Simplify $\sum_{k=1}^n \ln(k)$
Use log-of-product:
- $\sum_{k=1}^n\ln(k)=\ln\!\left(\prod_{k=1}^n k\right)=\ln(n!)$

So:
- $\boxed{\sum_{k=1}^n\ln(k)=\ln(n!)}$

(Asymptotically: $\ln(n!)=\Theta(n\ln n)$.)

---

#### 6) Analogy (why logs help)
Logs are like a “complexity compressor”:
- products become sums,
- powers become multipliers.
So complicated multiplicative expressions become manageable.

---

### 🧠 Aufgabe 3: Number of Perfect Matchings

Question:
How many possible perfect pairings are there with $n$ applicants and $n$ jobs?  
(Important: not asking how many are stable, just total perfect pairings.)

Decision process:
- applicant 1 has $n$ choices,
- applicant 2 has $n-1$ remaining choices,
- ...
- applicant $n$ has $1$.

Multiply:
- $n(n-1)\cdots 1=n!$

Final:
- $\boxed{n!}$

---

#### Analogy
Assigning $n$ distinct people to $n$ distinct seats = counting permutations = $n!$.

---

### ✅ Final compact answers (exam-style)

1. **Aufgabe 1a**  
   Naive unstable-pair swapping is not guaranteed to terminate, because depending on unstable-pair choices it can revisit previous states (cycle).

2. **Aufgabe 1b**  
   A stable matching is:
   - $\boxed{Z^*=\{(p_2,a_3),(p_1,a_2),(p_3,a_1)\}}$  
   It can be reached by naive algorithm for some choices, but not guaranteed.

3. **Aufgabe 2a**
   - $\log_a(x)=\dfrac{\log_b(x)}{\log_b(a)}$

4. **Aufgabe 2b**
   - $\log_a(x^y)=y\log_a(x)$

5. **Aufgabe 2c**
   - $4^{\log_2 n}=n^2$

6. **Aufgabe 2d**
   - $\dfrac{\ln(n^{\ln n})}{(\ln n)^2}=1$

7. **Aufgabe 2e**
   - $\sum_{k=1}^n\ln(k)=\ln(n!)$

8. **Aufgabe 3**
   - $\boxed{n!}$

---

*Status update: Präsenzblätter completed from 13 down to 1 in tutoring style with decisions, justifications, and analogies.*
