## 🔁 Replacement Block — Präsenzblatt 13 (with analogy + decision logic)

## 🏗 Topic 1 (Revised): Heap Construction Runtime and Correctness (Präsenzblatt 13, Aufgabe 1)

### 🧠 Big-picture analogy: The Emergency Room Pyramid
Imagine a hospital with a strict emergency hierarchy
- **Top doctor** (root) must always have highest priority patient.
- **Each doctor** supervises two juniors (children).
- **Rule**: every supervisor’s priority score must be at least as high as each junior's.
That is exactly a **max-heap**.

Building a heap is like reorganizing the hospital quickly so every supervisor-junior relation follows policy.

---

### 📚 Abbreviations and concepts
- **Heap**: complete binary tree with heap property.
- **Max-Heap**: every parent $\ge$ children.
- **trickle-down / sift-down**: repeatedly swap a node downward with larger child until local heap rule holds.
- **Loop invariant**: statement that stays true at each loop step.
- **$O(\cdot)$**: asymptotic upper bound.

---

### 🧭 What the exercise asks
1. Why recursive heapify (left, right, then trickle-down self) is $O(n)$.
2. Continue bottom-up build from given array/tree and answer where $i$ should start.
3. Prove bottom-up build is $O(n)$ using the loop invariant.

---

### ✅ Part (a): Why recursive heapify is $O(n)$

#### Decision step
We do **height-based counting** instead of naive “$n$ nodes × $\log n$ each”.
Why? Because most nodes are near leaves and barely move.

#### Theory tie-in
- A node at height $h$ can move down at most $h$ levels.
- Number of nodes at height $h$ is about $n/2^{h+1}$.

So total work:
\[
\sum_{h\ge0}\frac{n}{2^{h+1}}\cdot O(h)
=O\!\left(n\sum_{h\ge0}\frac{h}{2^h}\right)=O(n)
\]
since $\sum h/2^h$ converges.

\[
\boxed{\text{Recursive heapify build is }O(n)}
\]

#### How to think in exam
If you see heap-build runtime, think:
> “Count by height, not by nodes uniformly.”

---

### ✅ Part (b): Bottom-up trace + where to start $i$

Given initial array:
\[
[4,1,5,5,2,6,7,3,4,6,8,3]
\]

#### Decision: start index
Start at **last internal node**:
\[
i_{\text{start}}=\left\lfloor\frac{n}{2}\right\rfloor-1
\]
For $n=12$:
\[
i_{\text{start}}=5
\]

#### Why this works
Nodes after index 5 are leaves. Leaves already satisfy heap property (no children to violate rule), so running trickle-down there is unnecessary.

#### Step-by-step with justification
1. $i=5$ (value 6), child is 3 → already valid.
   - Why: parent ≥ child.
2. $i=4$ (2), children 6 and 8 → swap with 8.
   - Why: to restore max-parent rule, must swap with larger child.
3. $i=3$ (5), children 3 and 4 → valid.
4. $i=2$ (5), children 6 and 7 → swap with 7.
5. $i=1$ (1), children 5 and 8 → swap with 8; continue trickle-down with children 6 and 2 → swap with 6.
6. $i=0$ (4), children 8 and 7 → swap with 8; continue with children 5 and 6 → swap with 6.

Final heap:
\[
\boxed{[8,6,7,5,4,6,5,3,4,1,2,3]}
\]

Answer to “Wo könnte man $i$ beginnen lassen?”:
\[
\boxed{i=\lfloor n/2\rfloor-1}
\]

---

### ✅ Part (c): Correctness + runtime with loop invariant

Invariant given:
- At start of iteration $i$, all positions $n-1,\dots,i+1$ are roots of heaps.

#### Why this invariant is smart
It exactly matches bottom-up direction: by the time we handle $i$, both children are already “good heaps”.

#### Proof structure
- **Initialization**: True vacuously for highest indices (leaves).
- **Maintenance**: At iteration $i$, children indices are $>i$, so they root heaps by invariant. Trickle-down at $i$ merges them into heap rooted at $i$. Invariant preserved.
- **Termination**: after finishing $i=0$, root 0 is heap root for whole array.

So correctness is proven.

Runtime:
same height-counting sum:
\[
\sum_{h\ge0}\frac{n}{2^{h+1}}\cdot O(h)=O(n)
\]

\[
\boxed{\text{Bottom-up build is correct and }O(n)}
\]

---

### ⚠️ Common exam traps
1. Writing $O(n\log n)$ without height grouping.
2. Starting from $i=n-1$ as if all nodes need trickle-down.
3. Forgetting “swap with larger child” rule.

---

### 🧩 Micro-summary
- Heap-build is linear because expensive moves happen only at few high nodes.
- Start at last internal node.
- Invariant proof gives correctness cleanly.

---

## 🌊 Topic 2 (Revised): Residual Network (Präsenzblatt 13, Aufgabe 2)

### 🧠 Big-picture analogy: Water Pipes with Undo Buttons
Think of flow as water through pipes.
- Forward residual = free capacity left in pipe.
- Backward residual = how much water you can “undo/reroute” by reducing current flow.
Residual graph is your **live control panel** of what can still be changed.

---

### 📚 Abbreviations
- **Residual network** $G_f$: graph of remaining forward/backward capacities.
- **Augmenting path**: path from $s$ to $t$ in $G_f$ with all residual capacities > 0.
- **Bottleneck**: smallest residual on that path.

---

### 🧭 Decision strategy
For every original edge $(u,v)$ with current flow/capacity $f/c$:
- add forward residual $(u,v)$ with $c-f$ if positive,
- add backward residual $(v,u)$ with $f$ if positive.
Then search $s\to t$ path.

---

### ✅ Result and why it works
A valid augmenting path is:
\[
\boxed{s\to4\to5\to t}
\]
Residuals:
- $s\to4$: $15-1=14$
- $4\to5$: $5-0=5$
- $5\to t$: $9-0=9$

Bottleneck:
\[
\min(14,5,9)=5
\]

So yes, augmenting path exists:
\[
\boxed{\text{Flow can be increased by }5}
\]

#### How to think in exam
If any $s\to t$ path exists in residual graph, current flow is not maximum yet.

---

### 🧩 Micro-summary
Residual network = “what can still be pushed or undone”.
Path $s\to4\to5\to t$ proves current flow can still increase.

---

## 🔁 Replacement Block — Präsenzblatt 12 (with analogy + decision logic)

## 🏝 Topic 3 (Revised): MST + Connected Components (Präsenzblatt 12)

### 🧠 Big-picture analogy: Connecting Islands Cheaply
You own islands and want roads/bridges with minimum total cost.
- **MST** is the cheapest way to connect all islands without wasteful loops.
- A loop is like building an unnecessary extra bridge in a triangle.

---

### 📚 Abbreviations
- **MST** = Minimum Spanning Tree
- **Prim** = grow one connected kingdom from a start node
- **Kruskal** = pick globally cheapest edges, skip those creating cycles
- **ZHK** = Zusammenhangskomponente (connected component)

---

### 🧠 Aufgabe 1: MST via Kruskal and Prim

#### Decision strategy
Use Kruskal first:
- easy to audit (global sorted edges),
- clear cycle decision each step.
Then cross-check with Prim total weight.

#### Kruskal solution with why each step works
Pick edges in increasing weight, skipping cycle creators:
\[
(C,D,0),\ (D,E,1),\ (B,E,2),\ (A,B,3),\ (C,G,4),\ (G,H,5),\ (B,F,7)
\]
Why valid:
- each chosen edge connects two previously disconnected components,
- no cycle introduced,
- after 7 edges (for 8 nodes) tree is complete.

Total:
\[
0+1+2+3+4+5+7=\boxed{22}
\]

#### Prim confirmation
Starting from any node and always taking cheapest frontier edge gives an MST with same minimum total:
\[
\boxed{22}
\]

---

### ⚠️ Exam traps (MST)
1. In Kruskal, forgetting cycle check.
2. In Prim, taking globally cheapest edge instead of cheapest frontier edge.
3. Not ending with exactly $|V|-1$ edges.

---

### 🧠 Aufgabe 2: Find connected components in $H$ in $O(|V|+|E|)$

### Analogy: Paint Buckets on a Map
Pick an unpainted island, paint it and all reachable islands with same color via allowed roads.
Each color = one connected component.

---

### Decision strategy
Use BFS/DFS:
- only follow edges marked “belongs to $H$”.

### Why it works
- BFS/DFS from a start vertex reaches exactly vertices connected to it in $H$.
- Starting fresh only from unvisited nodes finds new components exactly once.

### Runtime justification
- each vertex visited once: $O(|V|)$
- each adjacency examined constant times: $O(|E|)$
Total:
\[
\boxed{O(|V|+|E|)}
\]

---

### 🧩 Micro-summary
- Kruskal/Prim both give MST weight 22.
- Components in subgraph $H$ via filtered BFS/DFS are linear-time.

---
