## 🔁 Replacement Block — Präsenzblatt 13 (Deep-Theory Version)

## 🏗 Topic 1 (Revised): Heap Build Runtime + Correctness (Präsenzblatt 13, Aufgabe 1)

### 🧠 What is asked?
You must solve three parts:
1. Show recursive heapify version runs in $O(n)$.
2. Continue bottom-up heap build on given insertion array and state where index $i$ should start.
3. Prove bottom-up build is $O(n)$ using the given loop invariant.

---

### 📚 Theory (from zero but university-rigorous)

#### Heap basics
- A **binary heap** is a complete binary tree stored in an array.
- For index $i$:
  - left child: $2i+1$
  - right child: $2i+2$
  - parent: $\lfloor(i-1)/2\rfloor$
- **Max-heap property**: for each node $i$, key$(i)\ge$ key(children of $i$).

#### Why leaves matter
A leaf has no children, so heap property at that node is automatically true.  
This is why bottom-up algorithms start at last internal node, not at the last array cell.

#### Trickle-down correctness idea
If left and right child subtrees are already heaps, trickle-down at root restores heap property for the entire rooted subtree.

---

### 🌉 Analogy: Military chain-of-command
Think of each node as a commander with two sub-commanders.
Rule: commander must have rank at least as high as both sub-commanders.
Leaves are soldiers with nobody below them — automatically valid.
Bottom-up build is “fix local commanders from lowest level upward.”

---

### ✅ Part (a): recursive heapify is $O(n)$

Algorithm (conceptual):
1. heapify(left subtree)
2. heapify(right subtree)
3. trickle-down(current root)

#### Decision strategy
Use **height-sum analysis**:
- Cost per node depends on how far it can move downward.
- Node at height $h$ costs at most $O(h)$.
- Number of nodes at height $h$ is at most $\approx n/2^{h+1}$.

Total:
\[
\sum_{h\ge0}\frac{n}{2^{h+1}}\cdot O(h)
=O\!\left(n\sum_{h\ge0}\frac{h}{2^h}\right)
=O(n)
\]
since $\sum_{h\ge0} h/2^h$ converges.

\[
\boxed{\text{Runtime }=O(n)}
\]

#### Why this works conceptually
Most nodes are near leaves and almost never move.
Only very few nodes near root can move many levels.

---

### ✅ Part (b): execute bottom-up build + where to start $i$

Given array:
\[
[4,1,5,5,2,6,7,3,4,6,8,3]
\]
with $n=12$.

#### Where can $i$ start?
Last internal node:
\[
i_{\text{start}}=\left\lfloor\frac{n}{2}\right\rfloor-1=5
\]
So best starting index:
\[
\boxed{i=5}
\]

#### Step-by-step (descending $i$)
1. $i=5$ (value 6), child=3 → valid.
2. $i=4$ (2), children 6 and 8 → swap with 8.
3. $i=3$ (5), children 3 and 4 → valid.
4. $i=2$ (5), children 6 and 7 → swap with 7.
5. $i=1$ (1), children 5 and 8 → swap with 8; continue down with children 6 and 2 → swap with 6.
6. $i=0$ (4), children 8 and 7 → swap with 8; continue down with children 5 and 6 → swap with 6.

Final heap:
\[
\boxed{[8,6,7,5,4,6,5,3,4,1,2,3]}
\]

#### Why each swap choice is justified
Always swap with larger child:
- guarantees parent becomes as large as possible locally,
- prevents immediate re-violation by the other child.

---

### ✅ Part (c): prove correctness and $O(n)$ with loop invariant

Loop:
\[
\text{for }i=n-1,n-2,\dots,0:\ \text{trickle-down}(i)
\]

Given invariant:
> At start of iteration for $i$, all positions $n-1,\dots,i+1$ are roots of heaps.

#### Initialization
For large indices (especially leaves), rooted subtrees are trivially heaps.

#### Maintenance
Assume invariant true at start of $i$.
Children of $i$ have larger indices, so their subtrees are heaps.
Applying trickle-down at $i$ restores heap property at root $i$ while preserving child-heaps.
Hence now positions $n-1,\dots,i$ are roots of heaps.

#### Termination
After finishing $i=0$, root 0 is heap root over whole array.
So whole array is a valid heap.

#### Runtime
Same height-sum argument:
\[
\sum_{h\ge0}\frac{n}{2^{h+1}}\cdot O(h)=O(n)
\]
Thus:
\[
\boxed{\text{Bottom-up build is correct and runs in }O(n)}
\]

---

### ⚠️ Exam pitfalls
1. Claiming “$n$ calls × $O(\log n)$ so $O(n\log n)$” without tighter height counting.
2. Starting at $n-1$ as if leaves need fixing.
3. Forgetting invariant maintenance justification.

---

### 🧩 Micro-summary
- Correct start index: $\lfloor n/2\rfloor-1$.
- Final heap here: $[8,6,7,5,4,6,5,3,4,1,2,3]$.
- Both recursive and bottom-up heap building are $O(n)$ with proper proof.

---

## 🌊 Topic 2 (Revised): Residual Network + Augmenting Path (Präsenzblatt 13, Aufgabe 2)

### 🧠 What is asked?
Construct residual network and decide if augmenting path exists.

---

### 📚 Theory
Given edge $(u,v)$ with flow/capacity $f/c$:
- forward residual capacity: $c-f$ (how much more can be sent),
- backward residual capacity: $f$ (how much sent flow can be canceled/rerouted).

Residual graph $G_f$ contains edges with positive residual capacity.

**Ford-Fulkerson principle**:
- if an $s\to t$ path exists in $G_f$, flow is not yet maximal.

---

### 🌉 Analogy: Plumbing with undo valves
Forward edge = extra water still possible.
Backward edge = undo valve to reclaim already sent water and reroute it elsewhere.

---

### 🧭 Decision strategy
1. Translate each original edge to forward/backward residual edges.
2. Search $s\to t$ path in residual graph.
3. Path bottleneck = minimum residual capacity along path.

---

### ✅ Result for the given network
A valid augmenting path is:
\[
\boxed{s\to4\to5\to t}
\]
Residual capacities:
- $s\to4:\ 15-1=14$
- $4\to5:\ 5-0=5$
- $5\to t:\ 9-0=9$

Bottleneck:
\[
\min(14,5,9)=5
\]

So:
\[
\boxed{\text{Yes, an augmenting path exists; flow can increase by }5}
\]

---

### 🧩 Micro-summary
Residual network answers “what can still change right now”.
Existence of $s\to t$ path proves current flow is not maximum.

---

## 🔁 Replacement Block — Präsenzblatt 12 (Deep-Theory Version)

## 🏝 Topic 3 (Revised): MST + Connected Components in Subgraph (Präsenzblatt 12)

### 🧠 Aufgabe 1: Compute an MST with Prim and Kruskal

---

### 📚 Theory
- A **spanning tree** connects all vertices with no cycles.
- An **MST** minimizes total edge weight among all spanning trees.
- **Kruskal** uses global edge ordering + cycle rejection.
- **Prim** grows one tree from a start vertex using cheapest frontier edge.

#### Correctness principles
- **Cut property**: lightest edge crossing any cut is safe for MST.
- Kruskal and Prim repeatedly add safe edges, hence produce MSTs.

---

### 🌉 Analogy: Cheapest bridge network between islands
You want all islands connected with minimum budget and no redundant loops (loops = wasted bridges).

---

### 🧭 Decision strategy
1. Kruskal first for transparent global verification.
2. Prim second to confirm same optimum (possibly different edge order).

---

### ✅ Kruskal solution
Chosen edges in ascending valid order:
\[
(C,D,0),\ (D,E,1),\ (B,E,2),\ (A,B,3),\ (C,G,4),\ (G,H,5),\ (B,F,7)
\]
Why valid:
- each edge connects different components at selection time,
- no cycle created,
- exactly $|V|-1=7$ edges for 8 vertices.

Total:
\[
0+1+2+3+4+5+7=\boxed{22}
\]

So MST weight:
\[
\boxed{22}
\]

---

### ✅ Prim solution
Starting from any node (e.g., $A$ or $C$), repeatedly take cheapest frontier edge.
A valid Prim run also yields an MST of total:
\[
\boxed{22}
\]

(Edge set may differ in tie cases, weight remains optimal.)

---

### ⚠️ Exam pitfalls
1. In Prim, choosing globally cheapest instead of frontier cheapest.
2. In Kruskal, forgetting cycle checks.
3. Reporting fewer or more than $|V|-1$ edges.

---

### 🧠 Aufgabe 2: Find connected components (ZHKs) in $H$ in $O(|V|+|E|)$

Given:
- graph $G=(V,E)$ as adjacency lists,
- subgraph $H=(V,E')$ with edge-membership marks.

Need:
- compute connected components of $H$ in linear time.

---

### 📚 Theory
BFS/DFS explores exactly one connected component from start vertex.  
Applying BFS/DFS repeatedly from unvisited vertices enumerates all components.

---

### 🌉 Analogy: Paint-bucket map coloring
Start at one unpainted island and paint all reachable islands with color 1.
Next unpainted island starts color 2, etc.
Each color = one connected component.

---

### 🧭 Decision strategy
Use DFS/BFS and ignore edges not in $E'$.

Pseudo-logic:
1. Mark all vertices unvisited.
2. For each vertex $v$:
   - if unvisited, start DFS/BFS from $v$,
   - during traversal, follow only edges marked “in $H$”.
3. Vertices reached in one traversal form one ZHK.

---

### ✅ Why this is correct
- Soundness: traversal only follows $E'$, so reached vertices are connected in $H$.
- Completeness: any vertex connected to start in $H$ will be discovered by DFS/BFS.
- Partitioning: each vertex first discovered in exactly one traversal, so components are disjoint and complete.

---

### ✅ Runtime
- Vertex processing: $O(|V|)$
- Each adjacency entry tested constant times: $O(|E|)$
Total:
\[
\boxed{O(|V|+|E|)}
\]

---

### 🧩 Micro-summary
- MST weight is 22 (via Kruskal/Prim).
- ZHKs in subgraph $H$ are obtained by filtered DFS/BFS in linear time.

---
---

## 🔁 Replacement Block — Präsenzblatt 11 (deeper theory + analogy + decision logic)

## 🗺️ Topic 4 (Revised): Floyd-Warshall Table Reduction + Dijkstra Counterexamples (Präsenzblatt 11)

### 🧠 Part A — Floyd-Warshall with one table (Aufgabe 1)

#### What is asked?
Show why we may write in-place update
\[
d_{ij}\leftarrow \min(d_{ij},\, d_{ik}+d_{kj})
\]
instead of using separate tables $D^{(k-1)}$ and $D^{(k)}$.

---

### 📚 Core theory from zero (university level, compact)

- We have weighted directed graph with vertices $1,\dots,n$.
- Define:
  \[
  d^{(k)}_{ij}=\text{length of shortest }i\to j\text{ path using only intermediate vertices from }\{1,\dots,k\}.
  \]
- Recurrence:
  \[
  d^{(k)}_{ij}=\min\!\big(d^{(k-1)}_{ij},\ d^{(k-1)}_{ik}+d^{(k-1)}_{kj}\big).
  \]

Interpretation:
- either best path from $i$ to $j$ does not use $k$,
- or it uses $k$ once, splitting into $i\to k$ and $k\to j$.

---

### 🧭 Decision logic: why in-place should even be possible
Potential danger:
- if we overwrite entries too early, later calculations might use already-modified values and break correctness.
So we must justify that values needed in round $k$ are stable.

Key fact in round $k$:
\[
d^{(k)}_{ik}=d^{(k-1)}_{ik},\qquad d^{(k)}_{kj}=d^{(k-1)}_{kj}
\]
because allowing $k$ as intermediate cannot improve a path that already ends/starts at $k$ via itself in a beneficial way.

Therefore, using one matrix in round $k$ is safe.

---

### ✅ Formal correctness sketch
For fixed $k$, each update uses current entries $d_{ik}, d_{kj}, d_{ij}$.  
These are effectively still the $k-1$-stage values needed for the recurrence regarding pivot $k$.  
Hence each overwrite computes exactly $d^{(k)}_{ij}$.

So one-table version is equivalent to multi-table version.

\[
\boxed{\text{One table is correct for Floyd-Warshall.}}
\]

---

### 🌉 Analogy
Think of a travel agency progressively allowing transit through cities 1,2,3,…,k.  
In round $k$, you ask: “Is route $i\to j$ cheaper if I allow a stop at city $k$?”  
You can update your price sheet in place because prices to/from city $k$ for this round are already settled.

---

### ⚠️ Exam pitfalls
1. Saying “works in practice” without proving overwrite safety.
2. Forgetting definition of $d^{(k)}_{ij}$ with restricted intermediates.
3. Mixing up node set and edge set restrictions.

---

## 🧠 Part B — Dijkstra construction tasks (Aufgabe 2)

### What is asked?
For the given directed graph, choose edge weights so that:

1. Exactly one edge weight is negative, and Dijkstra from $s$ fails on at least one node.
2. All edge weights are negative, but Dijkstra still returns correct distances.
3. There exist multiple different shortest-path trees from $s$.

---

### 📚 Dijkstra prerequisite theorem
Dijkstra is guaranteed correct when all edge weights are nonnegative.  
Failure mechanism with negative edges:
- a node may be finalized too early,
- a later path with negative edge could improve it, but algorithm no longer revisits finalized nodes.

---

### ✅ (1) One negative edge, Dijkstra fails — explicit construction

Use edges (matching the sheet’s shape) with these weights:
- $s\to A = 1$
- $s\to D = 4$
- $A\to B = 2$
- $D\to B = -5$  (only negative edge)
- $B\to C = 2$
- $B\to A = 1$ (or omit if not present in your version)

Why Dijkstra fails (from $s$):
- It may finalize $A$ early with distance 1.
- Later path $s\to D\to B\to A$ can become $4-5+1=0$, better than 1.
- But finalized $A$ is not corrected in standard Dijkstra.

So condition satisfied:
\[
\boxed{\text{one negative edge and wrong result possible}}
\]

---

### ✅ (2) All edges negative, Dijkstra still correct — construction

Make graph a pure outward arborescence from $s$ (no alternative competing routes), e.g.:
- $s\to A=-1,\ s\to D=-2,\ A\to B=-1,\ B\to C=-1$
and remove/backward alternatives that could create competing improvements.

Why Dijkstra still correct here:
- each reachable node effectively has a unique path from $s$.
- no alternative path exists to contradict a finalized distance.
- correctness here is accidental/structural, not theorem guarantee.

\[
\boxed{\text{All negative weights, but Dijkstra can still be correct on this instance}}
\]

---

### ✅ (3) Multiple shortest-path trees — construction

Set equal-cost alternatives, e.g.:
- $s\to A=1,\ s\to D=1,\ A\to B=1,\ D\to B=1,\ B\to C=1$

Then $B$ has two equal shortest predecessors ($A$ or $D$), producing different shortest-path trees with same distances.

\[
\boxed{\text{Multiple distinct shortest-path trees exist}}
\]

---

### 🌉 Analogy
Dijkstra is like sealing roads in a route plan once chosen.  
With negative “teleport” roads discovered later, sealed choices can become outdated.  
If network is a one-way tree, there is nothing to reconsider, so it can still work.

---

### 🧩 Micro-summary
- Floyd-Warshall one-table update is formally safe.
- Dijkstra can fail with negative edges.
- It may still succeed on special structures.
- Equal-cost predecessor choices yield multiple shortest-path trees.

---

## 🔁 Replacement Block — Präsenzblatt 10 (deeper theory + analogy + decision logic)

## 🎨 Topic 5 (Revised): Huffman Coding + Induction on deg(v) ≤ 2 (Präsenzblatt 10)

### 🧠 Part A — Huffman for “ESGIBTFREIBIER” (Aufgabe 1)

#### What is asked?
Construct an optimal prefix-free code and compute required bit count.

---

### 📚 Theory from zero
- **Prefix-free code**: no codeword is prefix of another.
- **Huffman algorithm**: repeatedly merge two least-frequent symbols.
- Optimality theorem: Huffman minimizes weighted external path length (expected code length).

---

### 🧭 Decision strategy
1. Count symbol frequencies.
2. Build Huffman tree bottom-up by repeatedly merging two smallest weights.
3. Assign 0/1 edges.
4. Compute total bits:
   \[
   \sum_{\text{symbol }x}\text{freq}(x)\cdot \text{codeLen}(x)
   \]

---

### ✅ Frequencies
Word: **ESGIBTFREIBIER**  
Counts:
- $E:3,\ I:3,\ R:2,\ B:2,\ S:1,\ G:1,\ T:1,\ F:1$

---

### ✅ One valid optimal Huffman code
(Exact bit patterns may vary; lengths and total bits must match optimum.)
Example:
- $E:00,\ I:01,\ R:100,\ B:101,\ S:1100,\ G:1101,\ T:1110,\ F:1111$

Lengths:
- $E,I$: length 2
- $R,B$: length 3
- $S,G,T,F$: length 4

Total bits:
\[
3\cdot2 + 3\cdot2 + 2\cdot3 + 2\cdot3 + 1\cdot4+1\cdot4+1\cdot4+1\cdot4
= 40
\]

\[
\boxed{40\text{ Bits}}
\]

---

### 🌉 Analogy
Frequent letters are like VIP passengers: they should get shortest boarding gates (shortest codes). Rare letters can walk longer routes.

---

### ⚠️ Exam pitfalls
1. Forgetting Huffman codes are not unique.
2. Comparing literal codewords instead of total weighted length.
3. Violating prefix-free property.

---

## 🧠 Part B — Graph components when deg(v) ≤ 2 (Aufgabe 2)

### What is asked?
For any connected component $Z$ in a finite undirected loop-free graph with $\deg(v)\le2$, show exactly one holds:
1. isolated vertex
2. simple path (at least 2 vertices)
3. simple cycle (at least 3 vertices)

---

### 📚 Theory basis
In undirected graphs:
- degree 0 → isolated
- degree 1 → endpoint behavior
- degree 2 → internal path node or cycle node

With max degree 2, branching is impossible (no node can split into 3 directions).

---

### 🧭 Decision strategy for proof
Use induction on number of vertices in a connected component.

- Base small sizes obvious.
- For induction step:
  - if component has a degree-0 node, it is isolated (size 1).
  - if it has degree-1 node, removing endpoint reduces to smaller component/path-like structure.
  - if all vertices degree-2, finite connected undirected graph must be a cycle.

---

### ✅ Proof skeleton (exam-ready)
- **Base case**: $|Z|=1$ ⇒ isolated node.
- **Induction hypothesis**: statement true for all connected components with fewer than $m$ vertices.
- **Step for $|Z|=m$**:
  - If some vertex has degree 1, remove it: remaining connected part satisfies IH and reattaching endpoint yields path.
  - If no vertex has degree 1:
    - either degree 0 (only possible for single node, already handled),
    - or all degree 2.
    - finite connected all-degree-2 undirected component is a simple cycle.
Thus exactly one type holds.

\[
\boxed{\text{Each component is isolated node, simple path, or simple cycle}}
\]

---

### 🌉 Analogy
Imagine each person can hold at most two hands:
- 0 hands: alone,
- two ends with one hand + middle with two hands: a line,
- everyone with two hands: a circle.
No one can form a branching “Y” shape.

---

### ⚠️ Exam pitfalls
1. Forgetting connected-component assumption in classification.
2. Confusing “at most degree 2” with “exactly degree 2”.
3. Not proving exclusivity (“exactly one”)—not just existence.

---

### 🧩 Micro-summary
- Huffman optimal coding here needs 40 bits.
- Degree bound $\le2$ forces components into only 3 geometric types.

---
---

## 🔁 Replacement Block — Präsenzblatt 9 (Deep-Theory + Algorithm-First + ADHD-Friendly)

## 🧱 Topic 6 (Revised): Matrix Chain Multiplication + Stable Cake Towers (Präsenzblatt 9)

---

### 🧠 Aufgabe 1: Matrix-Kettenmultiplikation (A1·A2·A3·A4)

Given dimensions:
- $A_1: 35\times 15$
- $A_2: 15\times 5$
- $A_3: 5\times 10$
- $A_4: 10\times 20$

Goal: find optimal parenthesization (minimum scalar multiplications).

---

### 📚 Theory from zero: What algorithm and why?
#### Algorithm: Dynamic Programming (DP) for Matrix Chain Order
When multiplying matrices, only parenthesization changes cost (final result matrix is the same).  
Brute force tries all parenthesizations (exponential).  
DP works because:
1. **Optimal substructure**: optimal solution for full chain contains optimal solutions for subchains.
2. **Overlapping subproblems**: same subchains appear repeatedly.

#### Cost formula
If split chain $A_i\cdots A_j$ at $k$:
\[
m[i,j]=m[i,k]+m[k+1,j]+p_{i-1}p_kp_j
\]
where dimension vector is:
\[
p=[35,15,5,10,20]
\]
($A_i$ has size $p_{i-1}\times p_i$).

---

### 🧭 Decision strategy (how to think in exam)
1. Build DP by increasing chain length $L=2,3,\dots,n$.
2. For each interval $(i,j)$, test all split positions $k$.
3. Keep minimum and remember split (for parentheses reconstruction).

---

### ✅ Step-by-step DP computation

Base:
\[
m[i,i]=0
\]

#### Length 2
- $m[1,2]=35\cdot15\cdot5=2625$
- $m[2,3]=15\cdot5\cdot10=750$
- $m[3,4]=5\cdot10\cdot20=1000$

#### Length 3
- $m[1,3]$:
  - split $k=1$: $0+750+35\cdot15\cdot10=6000$
  - split $k=2$: $2625+0+35\cdot5\cdot10=4375$
  - min $\Rightarrow m[1,3]=4375$ (split at 2)

- $m[2,4]$:
  - split $k=2$: $0+1000+15\cdot5\cdot20=2500$
  - split $k=3$: $750+0+15\cdot10\cdot20=3750$
  - min $\Rightarrow m[2,4]=2500$ (split at 2)

#### Length 4 (full chain)
- $m[1,4]$:
  - $k=1$: $0+2500+35\cdot15\cdot20=13000$
  - $k=2$: $2625+1000+35\cdot5\cdot20=7125$
  - $k=3$: $4375+0+35\cdot10\cdot20=11375$
  - minimum is $7125$ at $k=2$.

\[
\boxed{m[1,4]=7125}
\]

Optimal structure:
\[
\boxed{(A_1A_2)(A_3A_4)}
\]

---

### ✅ Why this works (correctness tie-in)
DP tries every legal split for each subchain exactly once and combines optimal sub-solutions by recurrence.  
By optimal substructure, the chosen minimum is globally optimal.

---

### 🌉 Analogy
Think of Lego plates:
- bad grouping creates huge intermediate plates (expensive),
- good grouping keeps intermediates small early.
DP is your “cost simulator” for all possible split points.

---

### ⚠️ Exam pitfalls
1. Using wrong dimensions in multiplication term.
2. Forgetting to try all split points $k$.
3. Returning minimum cost but not the parenthesization.

---

### 🧩 Micro-summary
- Use matrix-chain DP.
- Optimal parenthesization:
\[
\boxed{(A_1A_2)(A_3A_4)}
\]
- Minimum scalar multiplications:
\[
\boxed{7125}
\]

---

### 🧠 Aufgabe 2: Tatsächlich tragfähige Teiltortentürme (strictly descending layers)

We need number of representations of total size $n$ using forms $V$ (pairwise distinct), **with layers in descending order**.

---

### 📚 Theory from zero: Why old 1D DP is insufficient
Old recurrence:
\[
T(n)=\sum_{v\in V}T(n-v)
\]
counts ordered compositions (permutations), not descending-only stacks.

To encode sortedness without storing full sequences, enlarge state:
- amount already built,
- largest allowed next form size.

This is classic “coin change with order restriction”.

---

### 🧭 Algorithm choice
Use 2D DP:
\[
DP[s][j]=\text{#ways to build sum }s\text{ using only first }j\text{ form sizes}
\]
Assume sorted form sizes:
\[
v_1< v_2<\cdots<v_m
\]
Descending stack corresponds to choosing larger sizes earlier, equivalently counting multisets with restricted index progression.

Recurrence:
\[
DP[s][j]=DP[s][j-1] + DP[s-v_j][j-1]\quad\text{(if each size usable at most once)}
\]
or
\[
DP[s][j]=DP[s][j-1] + DP[s-v_j][j]\quad\text{(if reusable)}
\]
Choose variant according to exact interpretation of “Darstellungen” in your course (usually reusable pans ⇒ second formula).

Base:
- $DP[0][j]=1$
- $DP[s][0]=0$ for $s>0$

---

### ✅ Why this enforces sortedness
By restricting next choices to indices $\le$ current allowed index, you avoid counting reorderings separately.  
So “2+1+2” is never separate from “2+2+1”; only one canonical descending form remains.

---

### 🌉 Analogy
You are stacking boxes by size rule: each new box must be smaller than (or not larger than, depending strictness) previous.  
The second DP dimension is your “maximum box size still allowed”.

---

### ✅ Runtime
Let $m=|V|$.
- Time: $O(nm)$
- Space: $O(nm)$ (or optimized to $O(n)$ with rolling arrays)

---

### ⚠️ Exam pitfalls
1. Using 1D recurrence and accidentally counting permutations.
2. Not clarifying reusable vs non-reusable form sizes.
3. Forgetting base case $DP[0][*]=1$.

---

### 🧩 Micro-summary
- Need enlarged DP state to encode monotone-size constraint.
- Standard complexity remains polynomial:
\[
\boxed{O(n|V|)}
\]

---

## 🔁 Replacement Block — Präsenzblatt 8 (Deep-Theory + Algorithm-First + ADHD-Friendly)

## 🌳 Topic 7 (Revised): BST/AVL Checks + AVL Insertions + Cake Counting DP (Präsenzblatt 8)

---

### 🧠 Aufgabe 1a: Check BST property and AVL height condition for each tree

---

### 📚 Theory from zero

#### BST (Binary Search Tree) property
For each node with key $x$:
- all keys in left subtree $<x$
- all keys in right subtree $>x$

Must hold recursively for all nodes.

#### AVL condition
For each node:
\[
|h(\text{left})-h(\text{right})|\le 1
\]
This guarantees height $O(\log n)$ and therefore fast search/update.

---

### ���� Decision strategy
For each tree:
1. Check BST validity by interval logic (not only direct children).
2. Compute subtree heights bottom-up.
3. Check balance factor at every node.

---

### ✅ Tree checks (from sheet’s two trees)

#### First tree
Root 4; left subtree contains 1 with children 0,2; right subtree contains 5 with children 3,7 and descendants 6,8.

- BST check fails: node 3 appears in right subtree of 4 but $3<4$, violates BST global rule.
- AVL check: height differences at nodes stay within 1 (can verify bottom-up).
So:
- BST: **No**
- AVL: **Yes** (structurally balanced)

#### Second tree
Root 4; left subtree rooted at 2 with children 1,3 and 0 under 1; right subtree rooted at 6.

- BST: Yes (all interval constraints satisfied).
- AVL: check root: left height significantly larger than right (difference 2) → violation.
So:
- BST: **Yes**
- AVL: **No**

\[
\boxed{\text{Tree 1: not BST, AVL yes;\quad Tree 2: BST yes, AVL no}}
\]

---

### 🌉 Analogy
BST is like library shelf ordering by aisle ranges (global ordering rules).  
AVL is shelf-height safety: left and right shelf stacks must stay almost equal to avoid tipping.

---

### ⚠️ Exam pitfalls
1. Checking BST only with parent-child comparisons (insufficient).
2. Confusing “balanced at root” with “balanced everywhere”.

---

### 🧠 Aufgabe 1b: Insert sequence into empty AVL tree  
Sequence:
\[
1,0,5,4,3,2,6
\]

---

### 📚 Algorithm explained: AVL insertion
1. Insert like BST.
2. Walk back upward updating heights.
3. At first unbalanced node, inspect shape:
   - LL → right rotation
   - RR → left rotation
   - LR → left then right
   - RL → right then left

Why it works:
- rotations preserve BST inorder order,
- local restructuring restores balance factors,
- keeps height logarithmic.

---

### ✅ Step trace with decisions + justifications

1. Insert 1 → root.
2. Insert 0 → left of 1, balanced.
3. Insert 5 → right of 1, balanced.
4. Insert 4 → left of 5, still balanced globally.
5. Insert 3:
   - path: 1 → 5 → 4 → left
   - node 5 becomes LL-heavy (left-left pattern)
   - perform **right rotation at 5**
6. Insert 2:
   - now imbalance at node 1 with right-heavy subtree where insertion happened in left branch of right child (RL-type relative to 1 depending exact intermediate structure)
   - equivalent rebalancing leads to root becoming 4 after proper rotations (as in your previous trace)
7. Insert 6 → right side, balanced.

Final AVL (one valid final shape):
- root 4
- left subtree rooted at 2 (with 1 and 3; 0 under 1)
- right subtree rooted at 5 (with 6)

---

### 🧩 Micro-summary
AVL insertion = BST insert + local rotations.  
Goal is not “no imbalance ever”, but “repair immediately when factor exceeds 1”.

---

### 🧠 Aufgabe 2: Theoretische Teiltortentürme (count representations with forms 1,2,5)

Part (a): list possibilities for $n=5$.  
Part (b): algorithm for general $n$ and set $V$.

---

### 📚 Theory + algorithm choice
This is counting decompositions with allowed part sizes (classic DP).
For ordered representations:
\[
T(n)=\sum_{v\in V}T(n-v),\quad T(0)=1,\ T(x<0)=0
\]
For $V=\{1,2,5\}$ and $n=5$:
\[
T(5)=9
\]
(ordered sequences)

---

### ✅ Why recurrence works
Any valid representation of $n$ ends with exactly one last piece size $v\in V$.  
Removing that last piece gives unique representation of $n-v$.  
Summing over all possible last-piece choices gives full count.

---

### ✅ Bottom-up algorithm
Compute $T[0..n]$:
- $T[0]=1$
- for $i=1..n$:
  \[
  T[i]=\sum_{v\in V,\ i-v\ge0}T[i-v]
  \]
Return $T[n]$.

Runtime:
\[
\boxed{O(n|V|)}
\]
Space:
\[
\boxed{O(n)}
\]

---

### 🌉 Analogy
Video-game save points:
To solve level $n$, use already-solved counts for smaller levels $n-1,n-2,n-5$ instead of replaying from start.

---

### ⚠️ Exam pitfalls
1. Wrong base case ($T(0)$ must be 1).
2. Mixing ordered vs unordered interpretations.
3. Using recursion without memoization and timing out.

---

### 🧩 Micro-summary
- For $n=5$, $V=\{1,2,5\}$: 9 ordered representations.
- General counting solved via DP in $O(n|V|)$.

---
