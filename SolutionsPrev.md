---

## 🔁 Replacement Block — Präsenzblatt 13 (overwrite old Topic 1 + Topic 2 content logically)

## 🏗 Topic 1 (Revised): Heap Construction Runtime and Correctness (Präsenzblatt 13, Aufgabe 1)

### 🧠 What is asked?
You must handle three parts:

1. Show recursive `heapify(left), heapify(right), trickle-down(node)` runs in $O(n)$.
2. Continue bottom-up heap construction for given array/tree and answer where index $i$ can start.
3. Prove bottom-up build is $O(n)$ using the given loop invariant.

---

### 📚 Abbreviations used
- **Heap**: complete binary tree with heap property.
- **Max-Heap**: every parent $\ge$ children.
- **trickle-down / sift-down**: move node downward until heap property holds.
- **Loop invariant**: property true before each loop iteration.
- **$O(\cdot)$**: asymptotic upper bound.

---

### 🧭 Decision strategy
For runtime proofs on heaps:
1. Group nodes by **height**.
2. Cost per node at height $h$ is at most $h$.
3. Number of nodes at height $h$ is about $n/2^{h+1}$.
4. Sum over heights.

This is more robust than “root is expensive, leaves are cheap” intuition alone.

---

### ✅ Part (a): Why recursive heapify is $O(n)$

Algorithm idea:
- recursively heapify left and right subtrees,
- then trickle-down current node.

For each node at height $h$, trickle-down costs $O(h)$.
Count nodes per height:
- at most $\lceil n/2^{h+1}\rceil$ nodes at height $h$.

Total:
\[
T(n)\le \sum_{h\ge 0}\frac{n}{2^{h+1}}\cdot O(h)
= O\!\left(n\sum_{h\ge 0}\frac{h}{2^h}\right)
= O(n)
\]
since $\sum_{h\ge0} h/2^h$ converges (to 2).

So:
\[
\boxed{T(n)=O(n)}
\]

---

### ✅ Part (b): Bottom-up heap build trace + start index

Given initial array from the sheet:
- $[4,1,5,5,2,6,7,3,4,6,8,3]$

**Decision:** start at last non-leaf node:
\[
i_{\text{start}}=\left\lfloor \frac{n}{2}\right\rfloor-1
\]
For $n=12$:
\[
i_{\text{start}}=5
\]

Trace (descending $i$):
1. $i=5$: value 6, child 3 → already valid.
2. $i=4$: value 2, children 6 and 8 → swap with 8.
3. $i=3$: value 5, children 3 and 4 → valid.
4. $i=2$: value 5, children 6 and 7 → swap with 7.
5. $i=1$: value 1, children 5 and 8 → swap with 8, then swap down with 6.
6. $i=0$: value 4, children 8 and 7 → swap with 8, then swap down with 6.

Final max-heap array:
\[
\boxed{[8,6,7,5,4,6,5,3,4,1,2,3]}
\]

Answer to “Wo könnte man $i$ beginnen lassen?”:
\[
\boxed{i=\lfloor n/2\rfloor-1}
\]
(Starting later than this is possible, but this is the first index that can still have children.)

---

### ✅ Part (c): Loop invariant correctness + runtime

Loop:
- for $i=n-1,n-2,\dots,0$: trickle-down$(i)$

Given invariant:
- “At start of iteration for $i$, all positions $n-1,\dots,i+1$ are roots of heaps.”

**Initialization:**  
Before first relevant iteration (or vacuously for $i=n-1$), nodes beyond current index are leaves or already heap-roots.

**Maintenance:**  
Assume invariant holds at start of iteration $i$.  
Children of node $i$ are at indices $>i$, hence roots of heaps by invariant.  
Applying trickle-down at $i$ merges these into a heap rooted at $i$.  
So after iteration, positions $n-1,\dots,i$ are roots of heaps. Invariant preserved for next $i-1$.

**Termination:**  
After finishing $i=0$, index 0 is root of a heap containing all nodes.  
Therefore whole array is a heap.

**Runtime:**  
Same height-sum argument:
\[
\sum_{h\ge0}\frac{n}{2^{h+1}}\cdot O(h)=O(n)
\]
Hence:
\[
\boxed{\text{Bottom-up heap build runs in }O(n)}
\]

---

### ⚠️ Common exam pitfalls
1. Claiming build-heap is $O(n\log n)$ without height counting.
2. Starting from $i=n-1$ as if all are non-leaves.
3. Forgetting invariant maintenance argument needs “children already heap-roots”.

---

### 🧩 Micro-summary
- Build-heap bottom-up starts at $\lfloor n/2\rfloor-1$.
- Correctness follows from loop invariant.
- Runtime is linear because most nodes are low-height.
- Final array here: $[8,6,7,5,4,6,5,3,4,1,2,3]$.

---

## 🌊 Topic 2 (Revised): Residual Network (Präsenzblatt 13, Aufgabe 2)

### 🧠 What is asked?
Construct the residual network and decide whether an augmenting path exists.

---

### 📚 Abbreviations
- **Flow network**: directed graph with capacities.
- **Residual network** $G_f$: available forward/backward capacities under current flow.
- **Augmenting path**: path from source $s$ to sink $t$ in $G_f$ with positive residual capacity.
- **Bottleneck**: minimum residual capacity on that path.

---

### 🧭 Decision strategy
For every original directed edge $(u,v)$ with flow/capacity $f/c$:
- forward residual: $c-f$ on $(u,v)$
- backward residual: $f$ on $(v,u)$

Then run a path search from $s$ to $t$.

---

### ✅ Residual interpretation result
From the given instance, one valid augmenting path is:
\[
\boxed{s \to 4 \to 5 \to t}
\]
Residual capacities along this path:
- $s\to4$: $15-1=14$
- $4\to5$: $5-0=5$
- $5\to t$: $9-0=9$

Bottleneck:
\[
\min(14,5,9)=5
\]

So:
- augmenting path exists,
- additional flow possible = 5.

\[
\boxed{\text{Yes, there is an augmenting path.}}
\]

---

### 🧩 Micro-summary
Residual network = “what can still be pushed or undone.”  
Path $s\to4\to5\to t$ proves non-maximal flow; augment by 5.

---

## 🔁 Replacement Block — Präsenzblatt 12 (overwrite old Topic 3 content logically)

## 🏝 Topic 3 (Revised): MST + Connected Components in Subgraph (Präsenzblatt 12)

### 🧠 Aufgabe 1: MST via Prim and Kruskal

#### What is asked?
Compute one minimum spanning tree using:
1. Prim
2. Kruskal

---

### 📚 Abbreviations
- **MST** = Minimum Spanning Tree
- **Prim** = grows one connected tree from a start node
- **Kruskal** = sorts edges by weight and adds non-cycle edges
- **Cycle check** = usually DSU/Union-Find

---

### 🧭 Decision strategy
For exam stability:
- Do **Kruskal** first (easy to verify globally by sorted edges).
- Then sketch **Prim** to show same total weight (tree may differ in edge order, not total optimum).

---

### ✅ Kruskal result (from given graph)
Sorted useful edges lead to selecting:
\[
(C,D,0),\ (D,E,1),\ (B,E,2),\ (A,B,3),\ (C,G,4),\ (G,H,5),\ (B,F,7)
\]
This gives 7 edges for 8 nodes (tree complete, no cycle).

Total:
\[
0+1+2+3+4+5+7 = \boxed{22}
\]

So one MST has weight:
\[
\boxed{22}
\]

---

### ✅ Prim result (example run)
Start e.g. at $C$ and repeatedly add cheapest frontier edge.  
One valid Prim order can produce the same set (or another equivalent MST), total:
\[
\boxed{22}
\]

---

### ⚠️ Common pitfalls
1. Adding an edge that creates a cycle in Kruskal.
2. In Prim, taking cheapest global edge instead of cheapest frontier edge.
3. Stopping too early (need $|V|-1$ edges in connected graph).

---

### 🧩 Micro-summary
Both Prim and Kruskal return an MST of weight 22 for this graph.

---

### 🧠 Aufgabe 2: Find connected components of subgraph $H$ in $O(|V|+|E|)$

#### What is asked?
Graph $G=(V,E)$ stored with adjacency lists.  
Subgraph $H=(V,E')$ where each edge has a boolean “in $H$ or not”.  
Show connected components in $H$ can be found in linear time.

---

### 🧭 Decision strategy
Use DFS/BFS over all vertices, but traverse only edges marked “in $H$”.

---

### ✅ Algorithm
1. Mark all vertices unvisited.
2. For each vertex $v\in V$:
   - if unvisited: start DFS/BFS from $v$,
   - when exploring adjacency list, follow only edges whose mark says “in $H$”.
   - all reached vertices form one connected component.
3. Continue until all vertices visited.

---

### ✅ Correctness intuition
- DFS/BFS from $v$ reaches exactly vertices connected to $v$ through allowed edges in $E'$.
- Starting new search only from unvisited vertices enumerates each component exactly once.

---

### ✅ Runtime
- Vertex processing: $O(|V|)$
- Each adjacency entry checked constant-time once/twice overall: $O(|E|)$
Total:
\[
\boxed{O(|V|+|E|)}
\]

---

### 🧩 Micro-summary
Connected components in $H$ are found by standard DFS/BFS with edge-filter “is in $H$”, still linear time.

---
