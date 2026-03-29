## 🔁 Final Retrofitted Replacement — Präsenzblatt 13 (Operational, Blank-Slate, ADHD-Oriented)

## 🏗 Topic 1: Heaps from Zero — What Heapify Actually Does, Why It Works, Why Build-Heap is O(n)

---

### 🧠 What this sheet asks (plain language)
You are given two ways to build a max-heap and must show:
1. why a recursive heapify-based version is $O(n)$,
2. how bottom-up trickle-down proceeds on a concrete array,
3. why bottom-up is correct and $O(n)$ (with loop invariant).

So this is not just “run code” — it is:
- algorithm mechanics,
- correctness reasoning,
- runtime proof.

---

### 📚 Zero-to-functional foundations (not just labels)

#### 1) What is a heap and why do we care?
A **max-heap** is a data structure for fast “largest-element first” operations (priority queues).
It is:
- a **complete binary tree** (shape rule),
- with **heap property** (order rule): every parent key ≥ each child key.

Why this combination?
- shape rule lets us store it in a flat array.
- order rule guarantees global maximum at root (index 0).

---

#### 2) What does “complete binary tree” do for us?
Complete means:
- no random gaps,
- filled level by level from left to right.
That gives fixed index arithmetic:
- left child of $i$: $2i+1$
- right child of $i$: $2i+2$
- parent of $i$: $\lfloor(i-1)/2\rfloor$

So no pointers needed; array is enough.

---

#### 3) What exactly is `trickle-down` (sift-down)?
Use case:
- node at index $i$ may be too small versus children.
Goal:
- restore heap property in subtree rooted at $i$.

Procedure:
1. compare node with its children,
2. if node already ≥ both children: stop,
3. else swap with **larger child**,
4. continue from child position.

Why larger child?
Because parent must dominate both children after swap; choosing smaller child can immediately leave a violation with larger child.

---

#### 4) What exactly is `heapify` here?
In this sheet’s recursive variant, `heapify(i)` means:
1. first ensure left subtree is a heap (recursive call),
2. ensure right subtree is a heap (recursive call),
3. then run trickle-down at $i$ to make root consistent with those child-heaps.

So operationally:
- “child subproblems first, then local fix.”
This is a postorder-style divide-and-conquer repair.

---

### 🌉 Full analogy: Corporate promotion hierarchy audit
Imagine a company org chart:
- each manager must have performance score at least as high as direct reports.
If not, HR swaps manager downward with stronger report (trickle-down).
`heapify` at a manager means:
- first ensure both departments below are internally consistent,
- then fix this manager’s placement relative to those two departments.
Build-heap bottom-up means auditing low managers first, then higher ones.

This maps one-to-one to the algorithm.

---

## 🧠 Aufgabe 1a — Why recursive heapify build is O(n)

### Algorithm restated operationally
For each node as recursion unwinds:
- child subtrees have been heapified,
- do one trickle-down at this node.

### Runtime decision method
Do not multiply “n nodes × log n”. That overestimates.
Use **height-sensitive charging**:

- Node at height $h$ can trickle down at most $h$ levels.
- Number of nodes at height $h$ is about $n/2^{h+1}$.

Total work:
\[
\sum_{h\ge0}\frac{n}{2^{h+1}}\cdot O(h)
=O\!\left(n\sum_{h\ge0}\frac{h}{2^h}\right)
=O(n)
\]
since $\sum h/2^h$ converges.

\[
\boxed{O(n)}
\]

### Why this intuitively makes sense
Most nodes are near leaves (small $h$) and cost almost nothing.
Few top nodes are expensive, but there are very few of them.

---

## 🧠 Aufgabe 1b — Concrete bottom-up run + where to start i

Given array:
\[
[4,1,5,5,2,6,7,3,4,6,8,3],\quad n=12
\]

### Where can i start?
Last internal node:
\[
\left\lfloor\frac n2\right\rfloor-1 = 5
\]
Why not later?
Indices 6..11 are leaves; leaves can’t violate parent-child rule downward because they have no children.

\[
\boxed{i_{\text{start}}=5}
\]

### Step trace with reasoning

1. $i=5$, value 6, child 3 → already valid.
2. $i=4$, value 2, children 6 and 8 → swap with 8 (larger child).
3. $i=3$, value 5, children 3 and 4 → valid.
4. $i=2$, value 5, children 6 and 7 → swap with 7.
5. $i=1$, value 1, children 5 and 8 → swap with 8; continue with children 6 and 2 → swap with 6.
6. $i=0$, value 4, children 8 and 7 → swap with 8; continue with children 5 and 6 → swap with 6.

Final:
\[
\boxed{[8,6,7,5,4,6,5,3,4,1,2,3]}
\]

---

## 🧠 Aufgabe 1c — Correctness via loop invariant + O(n)

Loop invariant (given):
> At start of iteration for index $i$, all positions $n-1,\dots,i+1$ are roots of heaps.

### Why this invariant is the right lens
When processing $i$, both children are at larger indices, so invariant tells us child subtrees are already heaps. That is exactly precondition trickle-down needs.

### Proof skeleton

- **Initialization**: true for high indices (trivial for leaves).
- **Maintenance**: assume true at start of $i$.
  Children of $i$ are heap roots by invariant.
  Running trickle-down at $i$ restores heap rooted at $i$.
  Therefore range extends to include $i$.
- **Termination**: after $i=0$, whole array is one heap.

Hence correctness proven.

### Runtime
Same height-sum argument as above:
\[
\boxed{O(n)}
\]

---

### ⚠️ Pitfalls
1. Treating trickle-down cost uniformly as $\log n$ for every node.
2. Starting loop at wrong index.
3. Invariant proof without maintenance details.

---

### 🧩 Task-1 micro-summary
- `heapify` = heapify children, then trickle-down root.
- Bottom-up start index = $\lfloor n/2\rfloor-1$.
- Final heap matches computed array.
- Both build variants are linear-time overall.

---

## 🌊 Topic 2: Residual Networks from Zero — What They Do and Why Augmenting Path Matters

---

### 🧠 What this asks
Construct residual network from current flow/capacity labels and decide whether an augmenting path exists.

---

### 📚 Zero-to-functional foundations

#### 1) Flow network basics
Directed graph with:
- source $s$,
- sink $t$,
- capacities $c(u,v)$,
- current flow values $f(u,v)$.

Rules:
- $0\le f(u,v)\le c(u,v)$
- flow conservation at intermediate nodes.

---

#### 2) What residual network *does* operationally
Residual graph is the “control panel of possible next moves”.

For each original edge $(u,v)$:
- forward residual capacity:
  \[
  c_f(u,v)=c(u,v)-f(u,v)
  \]
  (extra flow still possible forward)
- backward residual capacity:
  \[
  c_f(v,u)=f(u,v)
  \]
  (how much current flow can be canceled/rerouted)

So residual graph encodes:
- where you can push more,
- where you can undo and redirect.

---

#### 3) Augmenting path and bottleneck
An augmenting path is any $s\to t$ path with positive residuals on all edges.
Bottleneck:
\[
\Delta=\min \text{ residual on path}
\]
You can increase total flow by exactly $\Delta$ along that path.

Key theorem:
- if augmenting path exists, current flow is not max.

---

### 🌉 Full analogy: Water network with reversible pumps
Think of each edge as a pipe.
Forward residual = empty room in pipe.
Backward residual = reversible pump that can pull water back to reroute.
Residual path from reservoir to city means “we still have a feasible way to send more water.”

---

### ✅ Apply to given network
One augmenting path:
\[
\boxed{s\to4\to5\to t}
\]
Residuals:
- $s\to4$: $15-1=14$
- $4\to5$: $5-0=5$
- $5\to t$: $9-0=9$

Bottleneck:
\[
\Delta=\min(14,5,9)=5
\]

So:
\[
\boxed{\text{Yes, augmenting path exists; increase flow by }5}
\]

---

### ⚠️ Pitfalls
1. Ignoring backward residual edges.
2. Using original capacities instead of residuals for path test.
3. Augmenting by more than bottleneck.

---

### 🧩 Task-2 micro-summary
Residual graph is not optional bookkeeping — it is the algorithmic state for deciding if and how flow can still improve.

---

## ✅ Final compact answers (exam-style)

1. Recursive heapify build is:
\[
\boxed{O(n)}
\]
2. Bottom-up starts at:
\[
\boxed{\lfloor n/2\rfloor-1}
\]
For given array, final heap:
\[
\boxed{[8,6,7,5,4,6,5,3,4,1,2,3]}
\]
3. Loop invariant proves bottom-up correctness and runtime:
\[
\boxed{O(n)}
\]
4. Residual network has augmenting path:
\[
\boxed{s\to4\to5\to t}
\]
with bottleneck:
\[
\boxed{5}
\]

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 12 (Operational, Blank-Slate, ADHD-Oriented)

## 🏝 Topic 3: MST + Connected Components — Full Conceptual-to-Formal Pipeline

---

### 🧠 What this sheet asks
1. Compute an MST using **both** Prim and Kruskal.
2. Show how to find connected components (ZHKs) in subgraph $H$ in $O(|V|+|E|)$.

---

### 📚 Zero-to-functional foundations (every term operational)

#### 1) Graph primitives
- **Vertex (node)**: object/location (city, island, computer).
- **Edge**: connection between two vertices.
- **Weighted edge**: edge has cost/length/time value.

Graph notation:
\[
G=(V,E)
\]

#### 2) Connectedness and cycles
- **Connected**: every vertex reachable from every other.
- **Cycle**: closed loop path.
- **Tree**: connected + acyclic.
- **Spanning tree**: tree containing all vertices.
- Any spanning tree on $|V|$ vertices has exactly $|V|-1$ edges.

#### 3) MST
**MST** = Minimum Spanning Tree: spanning tree with smallest total edge weight.

#### 4) Greedy algorithm
Makes local best choice each step.
For MST this is justified by cut-property safety.

#### 5) Prim and Kruskal operationally
- **Kruskal**: globally cheapest valid edges (cycle-free).
- **Prim**: cheapest frontier edge from currently grown tree.

#### 6) BFS/DFS abbreviations and mechanics
- **BFS** = Breadth-First Search (queue, layer by layer).
- **DFS** = Depth-First Search (stack/recursion, go deep first).
Both explore exactly one connected component from a start node.

#### 7) Subgraph
$H=(V,E')$ with $E'\subseteq E$.
Same vertices, only subset of edges active.

#### 8) ZHK
**ZHK = Zusammenhangskomponente** = connected component.

---

### 🌉 Full analogy set

#### MST analogy: island bridge budget
You have islands and possible bridge offers.
Need full connectivity with minimum total payment and no redundant loops.
Kruskal = central procurement sorted by price.
Prim = build outward from capital island using cheapest border bridge.

#### ZHK analogy: paint-bucket exploration
Take one unpainted island, paint all islands reachable via active bridges same color.
Each color is one component.
Repeat until all islands painted.

---

## 🧠 Aufgabe 1 — MST with Prim and Kruskal

### Algorithm 1: Kruskal (step-by-step)
1. Sort edges by ascending weight.
2. Start with no edges (all vertices separate components).
3. Add next cheapest edge only if it joins two different components.
4. Stop at $|V|-1$ edges.

Why step 3 works:
- if endpoints already connected, adding edge creates cycle (invalid for tree).

### Algorithm 2: Prim (step-by-step)
1. Start from arbitrary vertex.
2. Maintain current tree $S$.
3. Add minimum-weight edge crossing from $S$ to $V\setminus S$.
4. Repeat until all vertices included.

Why this works:
- lightest edge crossing a cut is safe (cut property).

---

### ✅ Result (from given graph)
A valid Kruskal MST:
\[
(C,D,0),\ (D,E,1),\ (B,E,2),\ (A,B,3),\ (C,G,4),\ (G,H,5),\ (B,F,7)
\]
Total:
\[
0+1+2+3+4+5+7=\boxed{22}
\]
Prim also yields MST of weight 22 (possibly different tie order).

---

### ⚠️ MST pitfalls
1. Prim frontier confusion.
2. Kruskal cycle omission.
3. Not checking $|V|-1$ selected edges.

---

## 🧠 Aufgabe 2 — ZHKs in $H$ in $O(|V|+|E|)$

### Algorithm
1. Mark all vertices unvisited.
2. For each vertex $v$:
   - if unvisited, run BFS/DFS from $v$,
   - traverse only edges marked active in $H$.
3. All reached vertices = one ZHK.
4. Continue until all visited.

### Why it works
- BFS/DFS discovers exactly reachable set from start via active edges.
- starting from each unvisited vertex enumerates all components exactly once.

### Runtime
- each vertex visited once: $O(|V|)$
- each adjacency scanned constant times: $O(|E|)$
Total:
\[
\boxed{O(|V|+|E|)}
\]

---

### ⚠️ ZHK pitfalls
1. Accidentally traversing inactive edges not in $E'$.
2. Forgetting to restart from every unvisited vertex.
3. Claiming linear runtime without adjacency-scan argument.

---

### 🧩 Micro-summary
- MST objective: cheapest full cycle-free connectivity.
- Prim/Kruskal are different greedy strategies with same optimal target.
- ZHKs found by BFS/DFS over active subgraph edges in linear time.

---

## ✅ Final compact answers (exam-style)

1. **MST (Prim/Kruskal):** total weight
\[
\boxed{22}
\]
2. **ZHKs in subgraph $H$:**
filtered BFS/DFS over active edges, runtime
\[
\boxed{O(|V|+|E|)}
\]

---

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 11 (Deep Algorithm Explanations + Blank-Slate)

## 🗺️ Topic 4: Floyd-Warshall (One-Table Proof) + Dijkstra Construction Tasks

---

### 🧠 Why this sheet matters
This sheet teaches two different shortest-path themes:
1. **All-pairs shortest paths** (Floyd-Warshall): shortest distances between every pair of nodes.
2. **Single-source shortest paths** (Dijkstra): distances from one start node.

It also teaches when an algorithm is valid vs when assumptions break.

---

## 🧠 Aufgabe 1: Why Floyd-Warshall can use one table

### 📚 Absolute-zero concept onboarding

#### What is a path?
A sequence of edges connecting start node to end node.

#### Path length (cost)
Sum of edge weights along the path.

#### Shortest path
Path with minimum total cost among all paths between two nodes.

#### All-pairs shortest paths
Need shortest-path distance for **every ordered pair** $(i,j)$.

---

### 📚 Floyd-Warshall algorithm explained from mechanism level

#### Key DP idea
We introduce intermediate vertices gradually.

Define:
\[
d^{(k)}_{ij}=\text{shortest path from }i\text{ to }j\text{ using only intermediates in }\{1,\dots,k\}.
\]

So stage $k$ means:
- “Now vertex $k$ is additionally allowed as a middle stop.”

Recurrence:
\[
d^{(k)}_{ij}=\min\Big(d^{(k-1)}_{ij},\ d^{(k-1)}_{ik}+d^{(k-1)}_{kj}\Big)
\]
Interpretation:
- either best path avoids $k$,
- or best path goes through $k$ exactly at split point.

---

### 🌉 Analogy
Think of planning train routes with stations unlocked one by one:
- At stage $k$, station $k$ becomes allowed as transfer station.
- For each origin/destination pair, ask:
  “Direct best so far, or cheaper if I transfer via station $k$?”

---

### 🧭 What the exercise asks exactly
Lecture first used $k$ separate tables, then claimed one table is enough.
You must justify in-place update:
\[
d_{ij}\leftarrow \min(d_{ij},\ d_{ik}+d_{kj})
\]
without breaking correctness.

---

### ✅ Why one table is safe (step-justified proof sketch)
Concern:
- if we overwrite entries too early, do later updates use “wrong stage” data?

Key fact in round $k$:
- values needed as helpers, $d_{ik}$ and $d_{kj}$, behave as stable references for this round’s logic.
- In standard loop ordering (fixed $k$, then all $i,j$), in-place update computes same recurrence outcome as separate-table version.

So each update still computes correct $d^{(k)}_{ij}$.

\[
\boxed{\text{One-table Floyd-Warshall is correct}}
\]

---

### Runtime
Three nested loops over vertices:
\[
\boxed{O(n^3)}
\]
Space:
- separate tables: larger
- in-place one table:
\[
\boxed{O(n^2)}
\]

---

### ⚠️ Pitfalls
1. Stating in-place “obviously works” without stage argument.
2. Forgetting meaning of $d^{(k)}_{ij}$.
3. Mixing APSP with single-source shortest paths.

---

## 🧠 Aufgabe 2: Dijkstra construction tasks (3 requested scenarios)

The sheet asks to assign edge weights so that:

1. Exactly one edge is negative and Dijkstra from $s$ fails.
2. All edges negative and Dijkstra still returns correct distances.
3. Multiple different shortest-path trees from $s$ exist.

---

### 📚 Dijkstra explained from zero

#### What Dijkstra solves
Single-source shortest paths in graphs with nonnegative edge weights.

#### Data structure intuition
Maintains tentative distances from source.
Repeatedly finalizes currently smallest tentative node.

#### Why nonnegative edges matter
When edge weights are nonnegative, once node has smallest tentative distance, no future detour can reduce it.  
Negative edges break this guarantee.

---

### 🌉 Analogy
Dijkstra is like sealing travel times city by city:
- once sealed, never revisited.
This is safe only if “future roads cannot magically refund cost” (no negative edges).

---

### ✅ (1) One negative edge, Dijkstra can fail
Provide weights so a node is finalized too early, then improved later through negative edge.

Example pattern:
- $s\to A=1$
- $s\to D=4$
- $A\to B=2$
- $D\to B=-5$  (only negative)
- $B\to C=2$

Then a route discovered later via $D\to B$ can invalidate earlier finalized assumption.

\[
\boxed{\text{Condition (1) achievable}}
\]

---

### ✅ (2) All edges negative, Dijkstra can still be correct on special structure
Construct graph so each node has essentially unique path from $s$ (no competing alternatives that could later improve finalized nodes).

Then even with negative weights, no contradiction appears during run.

\[
\boxed{\text{Condition (2) achievable for suitable structure}}
\]

---

### ✅ (3) Multiple shortest-path trees
Set equal-cost alternatives to some node (two different predecessors with same shortest distance contribution), e.g. symmetric edge weights.

Then different parent choices produce different shortest-path trees with same distances.

\[
\boxed{\text{Condition (3) achievable}}
\]

---

### ⚠️ Pitfalls
1. Confusing “Dijkstra theorem conditions” with “can never work otherwise.”
2. Not justifying failure mechanism in (1).
3. Giving only distances but not edge-weight construction logic.

---

### 🧩 Micro-summary
- Floyd-Warshall one-table update is correct and memory-efficient.
- Dijkstra requires nonnegative weights for guarantee.
- Constructive counterexamples/special cases explain exactly why.

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 10 (Deep Algorithm Explanations + Blank-Slate)

## 🎨 Topic 5: Huffman Coding + Degree-≤2 Component Classification by Induction

---

### 🧠 Why this sheet matters
You learn:
1. how to compress optimally with prefix codes (Huffman),
2. how to classify graph components rigorously with induction.

Both are core algorithmic thinking styles:
- greedy optimization,
- structural proof by induction.

---

## 🧠 Aufgabe 1: Optimal prefix-free code for “ESGIBTFREIBIER”

### 📚 Absolute-zero term onboarding

#### What is encoding?
Replacing symbols by bit strings.

#### What is variable-length coding?
Frequent symbols get short codes, rare symbols get longer codes.

#### What is prefix-free?
No codeword is prefix of another.
Why important:
- decoding is unambiguous while reading left-to-right.

#### What is Huffman algorithm?
A greedy algorithm that builds optimal prefix-free code from frequencies.

---

### 📚 Huffman algorithm — fully operational
1. Count frequency of each symbol.
2. Put each symbol as leaf node in min-priority queue by frequency.
3. Repeat until one node remains:
   - extract two least frequent nodes,
   - create new parent with weight = sum,
   - reinsert parent.
4. Label edges (e.g., left=0, right=1).
5. Codeword of symbol = bits along root-to-leaf path.

Why this algorithm works (high-level):
- In any optimal prefix-free tree, least frequent symbols can be placed deepest together.
- Greedy merge preserves optimal substructure.

---

### 🌉 Analogy
Airport boarding passes:
- passengers who appear constantly should have shortest gate routes,
- rare passengers can take longer corridors.
Huffman repeatedly pairs the rarest travelers into deeper shared corridors.

---

### ✅ Frequencies for ESGIBTFREIBIER
- $E:3,\ I:3,\ R:2,\ B:2,\ S:1,\ G:1,\ T:1,\ F:1$

One valid optimal code assignment:
- $E:00,\ I:01,\ R:100,\ B:101,\ S:1100,\ G:1101,\ T:1110,\ F:1111$

(Equivalent optimal variants possible.)

Total bits:
\[
3\cdot2 + 3\cdot2 + 2\cdot3 + 2\cdot3 + 1\cdot4+1\cdot4+1\cdot4+1\cdot4
=40
\]
\[
\boxed{40\text{ Bits}}
\]

---

### ⚠️ Pitfalls
1. Thinking exact bit patterns are unique (they are not).
2. Violating prefix-free condition.
3. Forgetting weighted sum when computing total bits.

---

## 🧠 Aufgabe 2: If deg(v) ≤ 2, each connected component is only one of 3 forms

Need to show each component is exactly one of:
1. isolated vertex,
2. simple path,
3. simple cycle.

---

### 📚 Absolute-zero term onboarding

#### Degree deg(v)
Number of edges incident to vertex $v$.

#### Connected component
Maximal set of vertices connected by paths.

#### Simple path
No repeated vertices.

#### Simple cycle
Closed path with no repeated vertices except start=end.

---

### 📚 Why this structure is plausible
If every vertex has at most 2 neighbors, branching like a “Y” shape is impossible (would need degree 3).  
So local restriction strongly limits global shapes.

---

### 🌉 Analogy
People forming hand-holding groups:
- max 2 hands per person.
Possible formations:
- alone,
- line,
- circle.
No branching tree possible without someone holding 3 hands.

---

### ✅ Induction proof skeleton (exam-ready)
Induct on number of vertices in a connected component.

- **Base**: 1 vertex ⇒ isolated node.
- **Step**:
  - if a degree-1 vertex exists, peeling it from endpoint keeps line-like structure.
  - if no degree-1 and graph connected with degree ≤2, then all vertices degree 2 (or size 1 case), forcing a cycle in finite connected undirected graph.
Hence only listed forms occur.

\[
\boxed{\text{Each component is isolated node, simple path, or simple cycle}}
\]

---

### ⚠️ Pitfalls
1. Forgetting “connected component” context.
2. Confusing “at most 2” with “exactly 2”.
3. Not arguing exclusivity of cases.

---

### 🧩 Micro-summary
- Huffman gives optimal prefix-free code; here total is 40 bits.
- Degree bound $\le2$ restricts components to exactly three structural types.

---

---

## 🔁 Corrected Final Retrofitted Replacement — Präsenzblatt 9 (Fully Blank-Slate)

## 🧱 Topic 6: Matrix Chain Multiplication + Stable Cake Towers (Präsenzblatt 9)

---

### 🧠 Why this sheet matters
You practice two key algorithm skills:
1. choosing best structure among many possibilities (matrix parenthesization),
2. counting valid constructions with constraints (cake towers).

Both use **dynamic programming**, so we first define all needed words from zero.

---

### 📚 Absolute-zero mini glossary (used throughout this block)

#### What is “brute force”?
Brute force means:
- try **all possible solutions**,
- compute answer for each,
- pick the best/valid one.
It is simple but often too slow when possibilities explode.

#### What are “Catalan numbers” (informal here)?
Catalan numbers count many “parenthesization-like” structures, including:
- number of ways to parenthesize a product of matrices.
For $n$ matrices, number of full parenthesizations is the Catalan number $C_{n-1}$, which grows very fast (roughly exponential scale for algorithmic feasibility concerns).

So brute force parenthesization quickly becomes impractical.

#### What is a “recurrence”?
A recurrence is a formula that defines a value using **smaller values of the same kind**.
Example idea:
- “answer for size $n$ = combination of answers for smaller sizes.”

#### What is dynamic programming (DP)?
DP is a method to solve recurrences efficiently by:
1. identifying subproblems,
2. storing their answers,
3. reusing them instead of recomputing.

#### What is a “DP state”?
A DP state is the exact information needed to describe one subproblem.
Example:
- “minimum cost for matrices from index $i$ to $j$.”
That pair $(i,j)$ is the state identifier.

---

## 🧠 Aufgabe 1: Matrix-Kettenmultiplikation (A1·A2·A3·A4)

Given:
- $A_1: 35\times15$
- $A_2: 15\times5$
- $A_3: 5\times10$
- $A_4: 10\times20$

Goal:
- find parenthesization with minimum scalar multiplications.

---

### 📚 Foundational concepts for this task

#### Matrix multiplication cost
If
\[
X\in\mathbb{R}^{a\times b},\ Y\in\mathbb{R}^{b\times c},
\]
then computing $XY$ costs:
\[
a\cdot b\cdot c
\]
scalar multiplications.

#### Why parenthesization matters
Even though matrix multiplication is associative (final result same), intermediate shapes differ, and costs differ.

---

### 🌉 Analogy
Imagine assembling furniture parts:
- final assembled cabinet is same,
- but assembly order can create huge temporary structures (expensive handling) or small ones (cheap).
We want cheapest assembly order.

---

### 🧠 Why brute force is bad here (now fully explained)
If we tried all parenthesizations:
- the number of possibilities is Catalan-growth,
- increases very fast with number of matrices,
- quickly infeasible for larger $n$.

So we need DP (polynomial-time) instead.

---

### 📚 DP algorithm explained operationally

#### DP state (what exactly we store)
\[
m[i,j]=\text{minimum multiplication cost for }A_iA_{i+1}\cdots A_j
\]
This is the subproblem “best way to multiply this interval.”

#### Base case
\[
m[i,i]=0
\]
(one matrix alone needs no multiplication).

#### Transition (recurrence)
If final split is between $k$ and $k+1$:
\[
m[i,j]=\min_{i\le k<j}\Big(m[i,k]+m[k+1,j]+p_{i-1}p_kp_j\Big)
\]
where
\[
p=[35,15,5,10,20].
\]

Meaning of three terms:
1. cost to compute left block,
2. cost to compute right block,
3. cost to multiply those two resulting matrices.

#### Fill order
Compute shorter intervals first (length 2, then 3, then 4...), because larger intervals depend on them.

---

### ✅ Step-by-step computation

Length 2:
\[
m[1,2]=35\cdot15\cdot5=2625,\quad
m[2,3]=15\cdot5\cdot10=750,\quad
m[3,4]=5\cdot10\cdot20=1000
\]

Length 3:
\[
m[1,3]=\min\{
0+750+35\cdot15\cdot10,\;
2625+0+35\cdot5\cdot10
\}
=\min\{6000,4375\}=4375
\]
\[
m[2,4]=\min\{
0+1000+15\cdot5\cdot20,\;
750+0+15\cdot10\cdot20
\}
=\min\{2500,3750\}=2500
\]

Length 4:
\[
m[1,4]=\min\{
0+2500+35\cdot15\cdot20,\;
2625+1000+35\cdot5\cdot20,\;
4375+0+35\cdot10\cdot20
\}
\]
\[
=\min\{13000,7125,11375\}=7125
\]

So:
\[
\boxed{\text{minimum cost }=7125}
\]
Optimal split at $k=2$:
\[
\boxed{(A_1A_2)(A_3A_4)}
\]

---

### ✅ Why this recurrence is correct (and what “recurrence” means again)
A recurrence expresses big problem via smaller ones.
Here:
- every full parenthesization has a final split position $k$,
- for each candidate $k$, best total cost = best-left + best-right + final multiplication,
- taking minimum over all $k$ must give global optimum.

That is why this recurrence matches the true optimal cost.

---

### Runtime
- number of states $(i,j)$: $O(n^2)$
- each tries up to $O(n)$ split points
\[
\boxed{O(n^3)\text{ time},\ O(n^2)\text{ space}}
\]

---

### ⚠️ Common pitfalls
1. Wrong dimension term in recurrence.
2. Filling DP in wrong order.
3. Confusing “same result matrix” with “same runtime.”

---

## 🧠 Aufgabe 2: Stable cake towers (descending-size constraint)

You must count ways to build total size $n$ from allowed sizes $V$, with sorted/descending stacking condition.

---

### 📚 Foundational concepts for this task

#### What is an “unconstrained recurrence”?
“Unconstrained” means we do **not** enforce ordering in chosen pieces.
Then a common recurrence is:
\[
T(n)=\sum_{v\in V}T(n-v),\quad T(0)=1,\ T(x<0)=0
\]
Interpretation:
- choose last piece size $v$,
- count ways to complete remaining size $n-v$.

#### Why that is not enough now
This counts different orders separately:
- 1+2+2 and 2+1+2 counted separately.
But if stack must be descending, order is restricted; many sequences become invalid or duplicates.

So we need stronger state info.

---

### 📚 DP state design for constraint handling

Sort sizes:
\[
v_1<\dots<v_m
\]

Define state:
\[
DP[s][j]=\text{#ways to make sum }s\text{ using only sizes }v_1,\dots,v_j
\]
Now state includes:
- remaining sum $s$,
- largest allowed size index $j$.
This prevents permutation overcount and enforces monotone construction logic.

---

### 🌉 Analogy
You stack discs:
- each next disc must be no larger than previous one.
You keep a “size permission level” $j$ telling which disc sizes are still allowed now.

---

### Transition (reusable-sizes version)
\[
DP[s][j]=DP[s][j-1]+DP[s-v_j][j]\quad(s\ge v_j)
\]
Meaning:
1. don’t use size $v_j$ at all, or
2. use at least one $v_j$, then continue with same allowed range.

Base:
- $DP[0][j]=1$ (one way to reach exact sum 0: choose nothing further)
- $DP[s][0]=0$ for $s>0$

(If your course interprets each size usable once, switch to $DP[s-v_j][j-1]$ in second term.)

---

### ✅ Why this recurrence is correct
At each state $(s,j)$, every valid solution falls into exactly one of two disjoint classes:
- solutions that do not use $v_j$,
- solutions that use $v_j$.
These classes are exhaustive and non-overlapping, so sum of counts is correct.

---

### Runtime
\[
\boxed{O(n|V|)\text{ time},\ O(n|V|)\text{ space}}
\]

---

### ⚠️ Common pitfalls
1. Reusing unconstrained 1D recurrence and overcounting.
2. Missing state dimension that encodes ordering constraint.
3. Wrong base case.

---

### 🧩 Micro-summary
- Matrix chain needs interval DP; optimum:
\[
\boxed{(A_1A_2)(A_3A_4),\ \text{cost }7125}
\]
- Stable cake stacking needs expanded DP state with size-index restriction.

---

## 🔁 Corrected Final Retrofitted Replacement — Präsenzblatt 8 (Fully Blank-Slate)

## 🌳 Topic 7: BST/AVL Checks + AVL Insertions + Cake DP (Präsenzblatt 8)

---

### 🧠 Why this sheet matters
You learn:
1. how to validate tree properties,
2. how self-balancing trees repair themselves,
3. how recurrence-based counting algorithms are built.

---

### 📚 Absolute-zero mini glossary

#### What is a tree?
A connected graph with no cycles.

#### What is a rooted tree?
One node designated as root; parent-child hierarchy.

#### What is a binary tree?
A rooted tree where each node has at most two children:
- left child,
- right child.

#### What is height of a node/tree?
Height(node) = number of edges on longest downward path to leaf.
Height(tree) = height(root).

---

## 🧠 Aufgabe 1a: Check BST and AVL conditions for each shown tree

### 📚 Definitions used

#### BST (Binary Search Tree) property
For every node with key $x$:
- all keys in left subtree are < $x$,
- all keys in right subtree are > $x$.
This must hold recursively (global interval consistency).

#### AVL condition
For every node:
\[
|h(left)-h(right)|\le1
\]

---

### How to check algorithmically

1. **BST check using intervals**  
   Each node gets allowed range $(L,U)$.
   - left child gets $(L,x)$,
   - right child gets $(x,U)$.
   Violation of range => not BST.

2. **AVL check using postorder heights**  
   Compute subtree heights bottom-up.
   At each node test balance factor.

---

### Results (for the two given trees)
- Tree 1: BST violated (global-order issue), AVL can still hold.
- Tree 2: BST holds, AVL violated at a high node due to height difference.

\[
\boxed{\text{Tree 1: BST no, AVL yes;\quad Tree 2: BST yes, AVL no}}
\]

---

### 🌉 Analogy
BST = strict filing system by number ranges.  
AVL = bookshelf balance rule so searching never degenerates into scanning long leaning chain.

---

## 🧠 Aufgabe 1b: Insert 1,0,5,4,3,2,6 into empty AVL tree

---

### 📚 AVL insertion algorithm from mechanics level

1. Insert key as in ordinary BST.
2. Move back upward to root:
   - update heights,
   - detect first node with balance factor outside [-1,1].
3. Determine shape:
   - LL, RR, LR, RL.
4. Apply corresponding rotation(s).

#### What is a rotation?
A local subtree restructuring operation that:
- restores balance locally,
- preserves inorder key order (so BST property remains).

---

### Why this algorithm works
- BST insertion puts key at correct sorted location.
- only ancestors on insertion path can become unbalanced.
- local rotations fix that imbalance without breaking sorted order.

---

### High-level trace outcome
After required rotations during sequence, one valid final AVL is:
- root 4
- left subtree rooted at 2 (with 1 and 3, and 0 below 1)
- right subtree rooted at 5 (with 6)

(Equivalent balanced variants possible depending on exact rotation timing conventions.)

---

### Runtime
AVL height is $O(\log n)$, so insert/search/update path length is logarithmic:
\[
\boxed{O(\log n)}
\]

---

## 🧠 Aufgabe 2: Theoretische Teiltortentürme (sizes 1,2,5)

### Part (a): list possibilities for $n=5$
Ordered counting gives 9 possibilities total.

### Part (b): algorithm
Use recurrence:
\[
T(0)=1,\quad T(x<0)=0,\quad
T(n)=\sum_{v\in V}T(n-v)
\]
and compute bottom-up DP table.

---

### What is this recurrence saying in plain words?
To build size $n$:
- choose last piece size $v$,
- remaining work is size $n-v$.
So total ways = sum over all legal last-piece choices.

---

### Why it is correct
Every valid construction has exactly one last piece, so decomposition is complete and non-overlapping.

---

### Runtime
\[
\boxed{O(n|V|)\text{ time},\ O(n)\text{ space}}
\]

---

### ⚠️ Pitfalls
1. Wrong base case $T(0)$.
2. Confusing ordered vs unordered interpretations.
3. Naive recursion explosion without DP memo/table.

---

### 🧩 Micro-summary
- Binary tree/BST/AVL distinctions are operationally different and must be checked separately.
- AVL insertion is BST insertion + local rotations.
- Cake counting recurrence is a classic DP pattern over smaller subproblems.

---
---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 7 (Fully Blank-Slate, Operational)

## 🎲 Topic 8: Hashing for Unsorted Partner Matching + 2-Universal Hash Functions (Präsenzblatt 7)

---

### 🧠 Why this sheet matters
You learn:
1. how to get expected linear-time lookup in unsorted data (hashing),
2. why randomization/universal hash families control collisions,
3. how to do modular-hash arithmetic correctly.

---

## 🧠 Aufgabe 2: Tanzparty reloaded (unsorted list, expected linear time)

Given:
- unsorted list $L$ of pairwise distinct heights $l_i$,
- target sum $h$,
- find as many disjoint pairs with $l_i+l_j=h$.

---

### 📚 Absolute-zero onboarding

#### What is a “sorted” list?
A list where values are in increasing/decreasing order.

#### Why unsorted changes everything
If list is unsorted, position gives no magnitude information.

#### What is the two-pointer method (old method)?
For sorted list:
- one pointer at left (small),
- one at right (large),
- compare sum and move one pointer deterministically.

Why it works there:
- moving left increases value,
- moving right decreases value,
so each move has predictable effect.

Why it fails on unsorted list:
- moving index does not predict value direction,
- no monotonic guarantee.

---

#### What is hashing?
Hashing maps keys (values) to table positions via a hash function:
\[
\text{index}=h(\text{key})
\]
used for fast insert/find/delete.

#### What is a hash table?
An array-based structure storing keys by hash index, with collision handling.

#### What is a collision?
Two different keys map to same index.

#### What does expected $O(1)$ mean?
Average over randomness (e.g., random hash choice) is constant-time per operation, though worst-case single operation can be larger.

#### What is universal hashing (informal functional view)?
Choose hash function randomly from a family where pairwise collision probability is provably small.
This avoids pathological deterministic collision patterns too often.

---

### 🌉 Analogy
You run a party check-in desk:
- each person with height $x$ needs partner $h-x$.
- desk has instant “seen already?” lookup by ID bin (hash table).
If needed partner is already checked in, pair them immediately.

---

### 📚 Algorithm explained operationally (one-pass hash pairing)

State:
- hash set/table `T` = currently seen but unpaired heights
- counter `pairs=0`

For each height $x$ in $L$:
1. compute complement $y=h-x$
2. if $y$ is in `T`:
   - form pair $(x,y)$
   - remove $y$ from `T`
   - `pairs++`
3. else:
   - insert $x$ into `T`

Return `pairs`.

---

### ✅ Why this algorithm is correct

#### Invariant
After processing first $k$ elements:
- `pairs` equals number of disjoint valid pairs already formed from those $k$,
- `T` contains exactly processed elements not yet paired.

#### Maintenance logic
- If complement exists, pairing now is always valid and safe (disjointness preserved by removal).
- If complement does not exist, current element must wait; storing it is necessary.

At end, all possible disjoint pairs found during scan are counted.

---

### Runtime
Each element triggers constant expected hash operations:
- one lookup + at most one insert/remove.
Total:
\[
\boxed{O(n)\ \text{expected time},\ O(n)\ \text{space}}
\]

---

### ⚠️ Pitfalls
1. Forgetting to remove matched partner (double-use bug).
2. Claiming deterministic worst-case $O(n)$ without assumptions.
3. Using two-pointer on unsorted input.

---

## 🧠 Aufgabe 3: 2-universal hash family computation

Family:
\[
h_a(x)=\sum_{i=0}^{r-1}a_ix_i \pmod p
\]
Given:
\[
p=11,\ a=(6,1,7,4),\ x=(1,3,3,7),\ y=(4,7,1,1).
\]

Need:
1. compute $h_a(x)$ and $h_a(y)$, check collision,
2. for $a(c)=(6,1,c,4)$ find all $c$ with collision.

---

### 📚 Absolute-zero onboarding for modular arithmetic

#### What does “mod 11” mean?
Two integers are equivalent mod 11 if they differ by multiple of 11.
Remainder after division by 11 is canonical representative in $\{0,\dots,10\}$.

#### Collision in this setting
A collision means:
\[
h_a(x)=h_a(y)
\]
(i.e., same remainder value).

---

### ✅ Part 1: Evaluate hashes

\[
h_a(x)=6\cdot1+1\cdot3+7\cdot3+4\cdot7
=58\equiv 3\pmod{11}
\]

\[
h_a(y)=6\cdot4+1\cdot7+7\cdot1+4\cdot1
=42\equiv 9\pmod{11}
\]

So:
\[
\boxed{h_a(x)=3,\ h_a(y)=9,\ \text{no collision}}
\]

---

### ✅ Part 2: Solve for collision parameter c

\[
a(c)=(6,1,c,4)
\]
Need:
\[
h_{a(c)}(x)\equiv h_{a(c)}(y)\pmod{11}
\]

Compute:
\[
h_{a(c)}(x)=6\cdot1+1\cdot3+c\cdot3+4\cdot7=37+3c
\]
\[
h_{a(c)}(y)=6\cdot4+1\cdot7+c\cdot1+4\cdot1=35+c
\]

Set equal:
\[
37+3c\equiv 35+c \pmod{11}
\Rightarrow 2c\equiv -2\equiv 9 \pmod{11}
\]

Inverse of 2 mod 11 is 6 (since $2\cdot6=12\equiv1$):
\[
c\equiv 9\cdot6=54\equiv10\pmod{11}
\]

\[
\boxed{c=10}
\]

---

### 🌉 Analogy
Modulo arithmetic is like a circular clock with 11 positions.
Different raw totals can land on same clock position (collision).

---

### ⚠️ Pitfalls
1. Arithmetic mistakes in modulo reduction.
2. Forgetting modular inverse in linear congruence.
3. Confusing “different keys” with “must be different hash values.”

---

### 🧩 Micro-summary
- Unsorted partner matching: hash complement lookup gives expected linear time.
- For given parameters: no initial collision, and collision occurs exactly for \(c=10\).

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 6 (Fully Blank-Slate, Operational)

## 📉 Topic 9: Lower Bound for Search + Median-of-3 Randomized Partition (Präsenzblatt 6)

---

### 🧠 Why this sheet matters
You learn:
1. how to prove impossibility/lower bounds (not just design algorithms),
2. how probability improves pivot quality in randomized selection/partition.

---

## 🧠 Aufgabe 1: Show comparison-based search in sorted list needs Ω(log n)

---

### 📚 Absolute-zero onboarding

#### What is a lower bound?
A statement of form:
- “No algorithm in this model can do better than X asymptotically.”

#### What is “comparison-based”?
Algorithm gets information only via comparisons (e.g., <, >, =), no arithmetic indexing tricks beyond that model.

#### Why sorted-list search has multiple outcomes
For target value:
- it may be at one of $n$ indices,
- or not in list at all.
So at least $n+1$ distinguishable outcomes.

---

### 📚 Decision tree method explained

Represent any comparison-based algorithm as binary tree:
- internal node = one comparison question,
- each branch = outcome of that question,
- each leaf = final algorithm output.

If tree has $L$ leaves, its height is at least:
\[
\lceil\log_2 L\rceil
\]
because binary branching doubles max leaves each level.

Here $L\ge n+1$, so worst-case comparisons:
\[
\ge \log_2(n+1)=\Omega(\log n)
\]

\[
\boxed{\text{Lower bound } \Omega(\log n)}
\]

---

### 🌉 Analogy
Twenty-questions game:
one yes/no question can at best halve candidates.
To isolate one option among $n+1$, you need about $\log_2(n+1)$ questions.

---

### ⚠️ Pitfalls
1. Forgetting “not found” outcome.
2. Proving only for one algorithm instead of model-wide.
3. Confusing lower bound (Ω) with upper bound (O).

---

## 🧠 Aufgabe 2: Randomized Partition with median of 3

Given $n\ge3$ distinct elements.
Instead of choosing one random pivot, choose 3 random distinct elements and use their median as pivot.

Need:
a) formula for pivot-rank probability $p_i$  
b) improvement factor for selecting exact median (limit as $n\to\infty$).

---

### 📚 Absolute-zero onboarding

#### What is “rank i”?
Element with rank $i$ is $i$-th smallest element in array.

#### What is randomized partition?
Pivot-based split:
- smaller elements left,
- larger elements right.
Pivot quality affects recursion depth in quicksort/quickselect-style algorithms.

---

### 📚 Deriving \(p_i\) step-by-step

For rank-$i$ element to be median of chosen 3:
- one chosen element must be from ranks $<i$: $(i-1)$ choices,
- one chosen element from ranks $>i$: $(n-i)$ choices,
- rank-$i$ element itself included.

Favorable 3-sets:
\[
(i-1)(n-i)
\]
Total equally likely 3-sets:
\[
\binom{n}{3}
\]

So:
\[
\boxed{
p_i=\frac{(i-1)(n-i)}{\binom{n}{3}}
}
\]

---

### ✅ Part (b): exact median probability improvement
Classic one-random-pivot:
\[
P(\text{exact median})=\frac1n
\]

Median-of-3 at middle rank \(i\approx n/2\):
\[
p_{\text{med}}\approx \frac{(n/2)(n/2)}{n^3/6}
= \frac{3}{2n}
\]

Ratio:
\[
\frac{(3/2n)}{(1/n)}=\frac32
\]

\[
\boxed{\text{Improvement factor }=1.5}
\]
(50% more likely to pick exact median asymptotically.)

---

### 🌉 Analogy
Choosing one random judge can be extreme.
Choosing 3 random judges and taking their middle opinion is more robust against extremes.

---

### ⚠️ Pitfalls
1. Using ordered triples instead of combinations.
2. Forgetting denominator \(\binom{n}{3}\).
3. Confusing exact-median probability with good-near-median probability.

---

### 🧩 Micro-summary
- Sorted search lower bound is \(\Omega(\log n)\) via decision trees.
- Median-of-3 pivot probability for rank \(i\):
\[
\frac{(i-1)(n-i)}{\binom{n}{3}}
\]
- Exact-median chance improves by factor \(1.5\) asymptotically.

---
---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 5 (Fully Blank-Slate, Operational)

## 📘 Topic 10: Polynomial Multiplication via DFT + Dice Probability Foundations (Präsenzblatt 5)

---

### 🧠 Why this sheet matters
You learn two different “thinking modes”:
1. Algebraic transform method (DFT) to speed up polynomial multiplication logic.
2. Discrete probability modeling from sample space to expectation.

---

## 🧠 Aufgabe 1: Polynomial multiplication with DFT

Given:
\[
A(x)=1+2x,\quad B(x)=3-x
\]
Need:
\[
C(x)=A(x)B(x)
\]
using DFT method with padded vectors:
\[
a'=(1,2,0,0),\quad b'=(3,-1,0,0)
\]

---

### 📚 Absolute-zero onboarding

#### What is a polynomial?
Expression of form:
\[
a_0+a_1x+a_2x^2+\cdots
\]
Coefficients are numbers \(a_i\).

#### What does multiplication do?
Polynomial multiplication is coefficient convolution:
each output coefficient combines products of input coefficients.
Naively this is nested summation logic.

#### What is DFT (Discrete Fourier Transform)?
For our purpose:
- It converts coefficient representation into value representation at special points (roots of unity).
Key property:
- multiplication becomes pointwise multiplication in value domain.

So workflow:
1. Evaluate both polynomials at special points.
2. Multiply values componentwise.
3. Transform back (inverse DFT) to get coefficients.

---

#### What are roots of unity?
Complex numbers \(\omega_n^k\) with:
\[
\omega_n=e^{2\pi i/n},\quad (\omega_n)^n=1
\]
For \(n=4\):
\[
\omega_4=i,\quad \{1,i,-1,-i\}
\]

#### Why padding to length 4?
If degree(A)=1 and degree(B)=1, product degree can be 2.
Need enough evaluation points to recover coefficients uniquely.
Length 4 is sufficient and matches task statement.

---

### 🌉 Analogy
Think of music equalizer modes:
- coefficient view = “raw notes”.
- DFT view = “frequency channels”.
In frequency space, mixing signals is easier (componentwise), then convert back.

---

### 📚 Algorithm (operational)
1. Build padded coefficient vectors.
2. Compute DFT values of each.
3. Multiply corresponding entries.
4. Apply inverse DFT to get product coefficients.

---

### ✅ Step-by-step

Using points \(1,i,-1,-i\).

For \(A(x)=1+2x\):
- \(A(1)=3\)
- \(A(i)=1+2i\)
- \(A(-1)=-1\)
- \(A(-i)=1-2i\)

\[
DFT(a')=(3,\ 1+2i,\ -1,\ 1-2i)
\]

For \(B(x)=3-x\):
- \(B(1)=2\)
- \(B(i)=3-i\)
- \(B(-1)=4\)
- \(B(-i)=3+i\)

\[
DFT(b')=(2,\ 3-i,\ 4,\ 3+i)
\]

Componentwise product:
- \(6\)
- \((1+2i)(3-i)=5+5i\)
- \(-4\)
- \((1-2i)(3+i)=5-5i\)

\[
DFT(c')=(6,\ 5+5i,\ -4,\ 5-5i)
\]

Inverse DFT gives:
\[
c'=(3,5,-2,0)
\]
So:
\[
\boxed{C(x)=3+5x-2x^2}
\]

Sanity check:
\[
(1+2x)(3-x)=3+5x-2x^2
\]
matches.

---

### ✅ Why this algorithm is correct
DFT and inverse DFT are linear inverses on this vector space.
Pointwise multiplication corresponds exactly to convolution of coefficients.
So transformed workflow is algebraically equivalent to polynomial multiplication.

---

### ⚠️ Pitfalls
1. Wrong root powers/signs with \(i^2=-1\).
2. Forgetting zero-padding.
3. Skipping inverse normalization details.

---

## 🧠 Aufgabe 2: Two dice probability

Alice throws red + green fair six-sided dice.
Let \(S\) be sum.

Need:
1. sample space,
2. disjointness + independence of \(S\ge6\) and \(S\le3\),
3. expected number in \(n\) double-throws where \(S\ge6\).

---

### 📚 Absolute-zero onboarding

#### What is sample space \(\Omega\)?
Set of all elementary outcomes.

Since dice are distinguishable (red, green), outcomes are ordered pairs:
\[
(r,g),\quad r,g\in\{1,\dots,6\}
\]
So:
\[
|\Omega|=36
\]

#### What is an event?
A subset of \(\Omega\).

#### Disjoint events
\(A,B\) disjoint if:
\[
A\cap B=\emptyset
\]
cannot happen together.

#### Independent events
\(A,B\) independent if:
\[
P(A\cap B)=P(A)P(B)
\]

#### Expected value for repeated Bernoulli trials
If success probability is \(p\) each trial and \(n\) trials:
\[
E[X]=np
\]

---

### 🌉 Analogy
Think of 36 equally likely lottery tickets (ordered dice outcomes).  
Events are just labeled ticket subsets.

---

### ✅ Step-by-step solution

Let:
\[
A=\{S\ge6\},\quad B=\{S\le3\}
\]

1) Sample space:
\[
\boxed{\Omega=\{(r,g)\mid r,g\in\{1,\dots,6\}\},\ |\Omega|=36}
\]

2a) Disjoint?
A sum cannot be both \(\ge6\) and \(\le3\), so:
\[
A\cap B=\emptyset
\]
\[
\boxed{\text{Yes, disjoint}}
\]

2b) Independent?
\[
P(A\cap B)=0
\]
Now:
- \(B\): sums 2 (1 way), 3 (2 ways) => \(3/36=1/12\)
- \(A\): complement of sums 2,3,4,5 where counts \(1+2+3+4=10\)
\[
P(A)=1-\frac{10}{36}=\frac{26}{36}=\frac{13}{18}
\]
\[
P(A)P(B)=\frac{13}{18}\cdot\frac{1}{12}=\frac{13}{216}\neq0
\]
So not independent.

\[
\boxed{\text{Disjoint but not independent}}
\]

3) For \(n\) double-throws:
\[
p=P(S\ge6)=\frac{13}{18}
\]
\[
E[X]=np=n\cdot\frac{13}{18}
\]
\[
\boxed{E[X]=\frac{13n}{18}}
\]

---

### ⚠️ Pitfalls
1. Treating dice as indistinguishable.
2. Thinking disjoint implies independent (usually false unless one event has prob 0).
3. Counting sum frequencies incorrectly.

---

### 🧩 Micro-summary
- DFT workflow gives \(C(x)=3+5x-2x^2\).
- Dice events: disjoint yes, independent no.
- Expected count in \(n\) trials: \(13n/18\).

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 4 (Fully Blank-Slate, Operational)

## 📘 Topic 11: Binary Search Recurrence, Karatsuba, Master Theorem (Präsenzblatt 4)

---

### 🧠 Why this sheet matters
You learn:
1. translating algorithm behavior into recurrences,
2. solving recurrences (Master theorem / recursion tree),
3. fast multiplication by divide-and-conquer (Karatsuba).

---

## 🧠 Aufgabe 1a: Binary search recurrence + runtime + Master parameters

---

### 📚 Absolute-zero onboarding

#### What is binary search?
Algorithm to find target in sorted array:
1. inspect middle element,
2. if target smaller, recurse left half,
3. if larger, recurse right half.

Each step keeps only one half.

#### What is a recurrence?
Equation defining runtime \(T(n)\) by smaller-size runtimes.

---

### 📚 Runtime derivation
At each step:
- one recursive subproblem of size \(n/2\),
- constant extra work for comparison/indexing.

So:
\[
T(n)=T(n/2)+\Theta(1),\quad T(1)=\Theta(1)
\]

Repeated halving gives about \(\log_2 n\) levels, each constant work:
\[
\boxed{T(n)=\Theta(\log n)}
\]

Master form:
\[
T(n)=aT(n/b)+\Theta(n^\alpha)
\]
Here:
\[
a=1,\ b=2,\ \alpha=0
\]

---

### 🌉 Analogy
Searching a word in a phonebook by opening middle page each time and discarding half the book.

---

## 🧠 Aufgabe 1b: Karatsuba for \(4711\cdot1337\)

---

### 📚 Absolute-zero onboarding

#### Why not schoolbook multiplication?
Schoolbook uses 4 half-size multiplications after split.

#### Karatsuba idea
Reduce 4 half multiplications to 3 by algebraic trick.

If:
\[
x=a\cdot 10^m+b,\quad y=c\cdot 10^m+d
\]
then:
\[
xy=ac\cdot10^{2m}+\big((a+b)(c+d)-ac-bd\big)\cdot10^m+bd
\]
Needs only:
- \(ac\),
- \(bd\),
- \((a+b)(c+d)\).

---

### 🌉 Analogy
Instead of calculating two cross terms separately, compute one “combined package” then subtract known parts.

---

### ✅ Compute with \(m=2\)
\[
4711=47\cdot100+11,\quad 1337=13\cdot100+37
\]
\[
ac=47\cdot13=611
\]
\[
bd=11\cdot37=407
\]
\[
(a+b)(c+d)=58\cdot50=2900
\]
Middle:
\[
2900-611-407=1882
\]
Recombine:
\[
xy=611\cdot10000+1882\cdot100+407
=6{,}298{,}607
\]

\[
\boxed{4711\cdot1337=6{,}298{,}607}
\]

---

## 🧠 Aufgabe 2: Master theorem recurrences

Given:
\[
T(1)=1,\ n\text{ power of 2}
\]

### 📚 Absolute-zero onboarding: What is the Master Theorem?

The **Master Theorem** is a shortcut to estimate runtime recurrences of the form:
\[
T(n)=a\,T(n/b)+f(n)
\]
where:
- \(a\): number of recursive subproblems,
- \(n/b\): size of each subproblem,
- \(f(n)\): extra work outside recursive calls (split/merge/etc).

Core comparison quantity:
\[
n^{\log_b a}
\]
This is the “recursion-tree workload baseline.”

You compare \(f(n)\) against \(n^{\log_b a}\):

1. **Case 1 (recursion dominates)**  
   If \(f(n)\) is asymptotically smaller (polynomially smaller), then
   \[
   T(n)=\Theta\!\left(n^{\log_b a}\right).
   \]

2. **Case 2 (balanced)**  
   If \(f(n)\) matches:
   \[
   f(n)=\Theta\!\left(n^{\log_b a}\right),
   \]
   then
   \[
   T(n)=\Theta\!\left(n^{\log_b a}\log n\right).
   \]

3. **Case 3 (combine-work dominates)**  
   If \(f(n)\) is polynomially larger (plus regularity condition), then
   \[
   T(n)=\Theta(f(n)).
   \]

---

### 🧭 How to use it in exam (fast workflow)
1. Identify \(a,b,f(n)\).
2. Compute \(\log_b a\).
3. Compare growth of \(f(n)\) with \(n^{\log_b a}\).
4. Pick case and write \(\Theta(\cdot)\) result.

---

### ⚠️ Important limitation
If \(f(n)\) is not in the theorem’s standard comparison-friendly form (or regularity conditions fail), use a recursion tree / substitution instead.
That is why in your sheet’s part (c), recursion-tree reasoning is safer.

### (a) \(\,T(n)=4T(n/2)+cn^2\)
\[
a=4,\ b=2,\ \alpha=2,\ \log_b a=2
\]
equal case:
\[
\boxed{T(n)=\Theta(n^2\log n)}
\]

### (b) \(\,T(n)=3T(n/2)+cn\)
\[
a=3,\ b=2,\ \alpha=1,\ \log_2 3\approx1.585
\]
recursion term dominates:
\[
\boxed{T(n)=\Theta(n^{\log_2 3})}
\]

### (c) \(\,T(n)=2T(n/2)+c\log_2 n\)

Not pure polynomial toll term; use recursion tree:
- level \(i\): \(2^i\) nodes
- per-node toll: \(\log(n/2^i)=\log n-i\)
- level toll: \(2^i(\log n-i)\)
Summing levels gives \(\Theta(n)\), leaves add \(\Theta(n)\).

\[
\boxed{T(n)=\Theta(n)}
\]

---

### ✅ Why these methods are valid
Master theorem applies to standard \(aT(n/b)+f(n)\) forms with growth comparison.
If \(f(n)\) is nonstandard (like \(\log n\)), recursion tree is safe fallback.

---

### ⚠️ Pitfalls
1. Wrong \(a,b,\alpha\) extraction.
2. Misusing Master theorem outside conditions.
3. Arithmetic mistakes in Karatsuba recombination.

---

### 🧩 Micro-summary
- Binary search: \(T(n)=T(n/2)+\Theta(1)=\Theta(\log n)\).
- Karatsuba product: \(6{,}298{,}607\).
- Recurrence answers:
  - \(\Theta(n^2\log n)\),
  - \(\Theta(n^{\log_2 3})\),
  - \(\Theta(n)\).

---

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 3 (Fully Blank-Slate, Operational)

## 📘 Topic 12: False Induction in Merge Sort + How to Make Any Comparison Sort Stable (Präsenzblatt 3)

---

### 🧠 Why this sheet matters
You practice:
1. spotting a **logically invalid proof** (not just wrong final answer),
2. transforming an algorithm property (stability) by design trick.

This is core theoretical maturity for exams.

---

## 🧠 Aufgabe 1: Why the “Merge Sort is O(n)” induction proof is wrong

---

### 📚 Absolute-zero onboarding

#### What is Merge Sort?
Divide-and-conquer sorting:
1. split array into two halves,
2. recursively sort both halves,
3. merge sorted halves in linear time.

#### Typical recurrence for runtime
\[
T(n)\le 2T(\lceil n/2\rceil)+cn
\]
with constant \(c>0\).

#### What is induction proof structure?
- **Base case (IA)**: show statement for smallest input.
- **Induction hypothesis (IV)**: assume statement true for smaller inputs.
- **Induction step (IS)**: prove statement for size \(n\) from hypothesis.

#### What is Big-O?
\(f(n)\in O(g(n))\) means \(f(n)\) grows no faster than constant multiple of \(g(n)\) eventually.

---

### 🌉 Analogy
Imagine claiming: “My monthly cost is always at most 1000€.”  
If each month includes unavoidable extra fee +100€, saying “still around 1000€” is not proof.  
You must show exact inequality closes with same constant budget.

---

### ✅ Where the faulty proof fails (precise logic)
Faulty step informally says:
- \(T(n)\le 2T(\lceil n/2\rceil)+cn\),
- by induction \(T(\lceil n/2\rceil)\in O(n)\),
- therefore whole expression in \(O(n)\).

Problem: this is too loose for induction closure.  
A proper linear induction must use fixed constant \(C\):

Assume for all \(k<n\):
\[
T(k)\le Ck
\]
Then:
\[
T(n)\le 2C\lceil n/2\rceil + cn \approx Cn+cn=(C+c)n
\]
To conclude \(T(n)\le Cn\), you'd need \(C+c\le C\), impossible for \(c>0\).

So the induction does not close. That is the exact error.

\[
\boxed{\text{Error is in induction step: Big-O used too loosely, constants not closed}}
\]

Correct asymptotic runtime:
\[
\boxed{T(n)=\Theta(n\log n)}
\]

---

### ⚠️ Pitfalls
1. Treating \(O(\cdot)\) as algebraic equality.
2. Ignoring constants in induction closure.
3. Concluding from “both terms are \(O(n)\)” without proper quantified bound.

---

## 🧠 Aufgabe 2: Every comparison-based sorting algorithm can be made stable

---

### 📚 Absolute-zero onboarding

#### What is sorting stability?
A sorting algorithm is **stable** if equal keys keep original relative order.

Example:
Input:
\[
(5,A),(3,B),(5,C)
\]
Stable output by key:
\[
(3,B),(5,A),(5,C)
\]
A remains before C.

#### What is comparison-based sorting?
Algorithm decides order using only pairwise comparisons (<, >, =), not counting/radix bucket logic.

---

### 📚 Construction method (algorithmic transformation)
Given elements \(x_1,\dots,x_n\), replace each by pair:
\[
(x_i,\ i)
\]
where \(i\) is original index.

Define comparator lexicographically:
\[
(x_i,i) < (x_j,j)
\iff
\big(x_i<x_j\big)\ \text{or}\ \big(x_i=x_j \land i<j\big)
\]

Run original comparison-based sorting algorithm on these pairs.

---

### ✅ Why this works
- If keys differ, original ordering logic unchanged.
- If keys equal, smaller original index wins.
So equal keys are forced to stay in input order.

Thus transformed algorithm is stable.

\[
\boxed{\text{Any comparison-based sorter can be made stable via index tie-break}}
\]

---

### Runtime impact
Comparator gets constant extra tie-break work.
Asymptotic class stays same (up to constant factor).

---

### 🌉 Analogy
Like ranking exam papers by score; ties are resolved by submission timestamp.
Equal scores now preserve arrival order automatically.

---

### ⚠️ Pitfalls
1. Forgetting to attach original index.
2. Using unstable tie-break rule.
3. Claiming transformation changes asymptotic complexity class significantly.

---

### 🧩 Micro-summary
- Merge-Sort false proof fails because induction constants do not close.
- Stability can be enforced by sorting pairs (key, original_position).

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 2 (Fully Blank-Slate, Operational)

## 📘 Topic 13: Landau Growth Ordering + Pair Matching in Sorted List (Präsenzblatt 2)

---

### 🧠 Why this sheet matters
You learn:
1. asymptotic growth comparison method (essential for runtime ranking),
2. a linear-time two-pointer algorithm with proof.

---

## 🧠 Aufgabe 1: Order functions by asymptotic growth

Given:
\[
f_1(n)=\pi n,\ 
f_2(n)=10^{1/n},\ 
f_3(n)=2^n,\ 
f_4(n)=n\ln n,\ 
f_5(n)=\log(n\log n),\ 
f_6(n)=\sqrt{n^3}=n^{3/2}
\]

Need ordering under:
\[
f\preceq g \iff f\in O(g)
\]

---

### 📚 Absolute-zero onboarding

#### What is asymptotic growth?
How a function behaves for very large \(n\), ignoring constant factors and lower-order details.

#### Landau symbols quick recap
- \(f\in O(g)\): \(f\) grows no faster than \(g\).
- \(f\in \Omega(g)\): \(f\) grows at least as fast as \(g\).
- \(f\in \Theta(g)\): same growth rate up to constants.

#### Growth hierarchy intuition
Typically:
\[
\text{constant} < \log n < n < n\log n < n^p\ (p>1) < a^n\ (a>1)
\]

---

### 🌉 Analogy
Think of six cars with different acceleration laws.
At first they may look similar, but on a very long highway their acceleration class determines final ordering.

---

### ✅ Step-by-step comparisons

1. \(f_2(n)=10^{1/n}\to 1\): constant class \(\Theta(1)\).
2. \(f_5(n)=\log(n\log n)=\log n+\log\log n\in\Theta(\log n)\).
3. \(f_1(n)=\pi n\in\Theta(n)\).
4. \(f_4(n)=n\ln n\).
5. \(f_6(n)=n^{3/2}\), compare with \(n\ln n\):
\[
\frac{n\ln n}{n^{3/2}}=\frac{\ln n}{\sqrt n}\to0
\Rightarrow n\ln n=o(n^{3/2})
\]
6. \(f_3(n)=2^n\): exponential, dominates polynomial/log classes.

Final order:
\[
\boxed{
10^{1/n}\prec \log(n\log n)\prec \pi n\prec n\ln n\prec n^{3/2}\prec 2^n
}
\]

---

### ⚠️ Pitfalls
1. Forgetting constants (\(\pi\)) do not change class.
2. Misreading \(\log(n\log n)\) as \( (\log n)(\log\log n)\).
3. Underestimating exponential growth.

---

## 🧠 Aufgabe 2: Pair matching in sorted list (sum = h)

Given:
- sorted list \(L=(l_1,\dots,l_n)\), all distinct,
- target \(h\),
- count how many disjoint pairs satisfy \(l_i+l_j=h\).

---

### 📚 Absolute-zero onboarding

#### What is the two-pointer algorithm?
Technique using two indices:
- left pointer \(i\) at smallest element,
- right pointer \(j\) at largest element.

Because list is sorted, pointer moves are logically meaningful:
- sum too small → increase smaller side (\(i++\)),
- sum too large → decrease larger side (\(j--\)),
- exact match → count pair and move both.

---

### 🌉 Analogy
Seesaw balancing game:
- left child = too light side,
- right child = too heavy side.
If balance too low, move left to heavier candidate.
If too high, move right to lighter candidate.

---

### 📚 Algorithm (explicit)
Initialize:
\[
i=1,\ j=n,\ \text{count}=0
\]
While \(i<j\):
- \(s=l_i+l_j\)
- if \(s=h\): count++, \(i++\), \(j--\)
- if \(s<h\): \(i++\)
- if \(s>h\): \(j--\)

Return count.

---

### ✅ Why algorithm is correct

Loop invariant idea:
At loop start, all pairs outside interval \([i,j]\) are already correctly decided.

Justification of moves:
- If \(l_i+l_j<h\), pairing \(l_i\) with any element \(\le l_j\) can’t reach \(h\). So discard \(l_i\) safely.
- If \(l_i+l_j>h\), pairing \(l_j\) with any element \(\ge l_i\) can’t drop to \(h\). So discard \(l_j\) safely.
- If equal, pair found; with distinct values and disjoint-pair counting, moving both is correct.

Termination at \(i\ge j\): no candidate pair remains.

---

### Runtime
Each step moves at least one pointer inward.
Each pointer moves at most \(n\) times:
\[
\boxed{O(n)\text{ time},\ O(1)\text{ space}}
\]

---

### ⚠️ Pitfalls
1. Applying two-pointer to unsorted list.
2. Forgetting disjointness assumption when counting.
3. Not proving pointer moves are safe eliminations.

---

### 🧩 Micro-summary
- Growth ordering solved via class hierarchy + limit/ratio checks.
- Sorted pair-sum counting solved in linear time via two-pointer elimination logic.

---

---

## 🔁 Final Retrofitted Replacement — Präsenzblatt 1 (Fully Blank-Slate, Operational)

## 📘 Topic 14: Correctness of Sequential Search + Basic Runtime Thinking (Präsenzblatt 1)

---

### 🧠 Why this first sheet matters
This is foundational algorithmics:
1. precisely describe a simple algorithm,
2. prove it is correct (not just “it seems to work”),
3. analyze runtime cleanly.

If this is clear, later proofs (DP, graphs, heaps) become much easier.

---

## 🧠 Aufgabe 1: Sequential search in an unsorted list

Typical task form:
- Given list \(L=(l_1,\dots,l_n)\),
- given target value \(x\),
- return an index \(i\) with \(l_i=x\), or report “not found”.

---

### 📚 Absolute-zero onboarding

#### What is an algorithm?
A finite, unambiguous step-by-step procedure that always terminates and returns output.

#### What is sequential (linear) search?
Check elements one by one from start to end until:
- target found, or
- list exhausted.

No sorting assumption needed.

#### Why use it?
It is the simplest guaranteed method when no structure (like sorted order or hash index) is given.

---

### 🌉 Analogy
Looking for a name in an unsorted stack of papers:
you inspect page 1, then 2, then 3, ... until found or no pages left.

---

### 📚 Algorithm (explicit)

Input: array/list \(L[1..n]\), target \(x\)

1. set \(i\leftarrow 1\)
2. while \(i\le n\):
   - if \(L[i]=x\), return \(i\)
   - else \(i\leftarrow i+1\)
3. return “not found”

---

### ✅ Correctness proof (clean exam version)

We prove:
- if algorithm returns index \(i\), then \(L[i]=x\),
- if algorithm returns “not found”, then \(x\) is not in list.

#### Loop invariant
At start of each iteration with current index \(i\):
\[
x \notin \{L[1],L[2],\dots,L[i-1]\}
\]
(i.e., all previously checked elements are not \(x\)).

#### Initialization
Before first iteration, \(i=1\), set \(L[1..0]\) is empty, invariant true.

#### Maintenance
Assume invariant holds at start of iteration \(i\).
- If \(L[i]=x\), algorithm returns correct index.
- Else \(L[i]\neq x\), increment \(i\). Then all checked elements up to old \(i\) are not \(x\), so invariant holds for next iteration.

#### Termination
Loop ends when \(i=n+1\).
By invariant, \(x\) is not in \(L[1..n]\), so “not found” is correct.

\[
\boxed{\text{Sequential search is correct}}
\]

---

### Runtime analysis

#### Worst case
Target absent or at last position:
- \(n\) comparisons.
\[
\boxed{O(n)}
\]

#### Best case
Target at first position:
\[
\boxed{O(1)}
\]

#### Average case (uniform position assumption + maybe absent model variant)
Typically linear expectation scale:
\[
\boxed{\Theta(n)}
\]
(up to constant factor details depending exact probabilistic model).

---

### ⚠️ Pitfalls
1. Forgetting to prove “not found” case.
2. Missing loop invariant.
3. Confusing correctness proof with runtime analysis.
4. Claiming logarithmic time without sorted data.

---

## 🧠 (Common companion task) Compare with binary search

If sheet also asks comparison:

### 📚 Binary search quick onboarding
Requires sorted list.
At each step:
- compare with middle,
- discard half.

Runtime:
\[
\boxed{O(\log n)}
\]
But only valid with sorted input.

Sequential search remains universally applicable without preprocessing.

---

### 🌉 Analogy
Binary search is like guessing a number with “higher/lower” feedback (halving possibilities).  
Sequential search is checking cards one by one in random pile.

---

## 🧩 Final micro-summary
- Sequential search checks elements in order and always works on unsorted input.
- Correctness proven via loop invariant.
- Runtime:
  - best \(O(1)\),
  - worst \(O(n)\),
  - average \(\Theta(n)\) (standard model).
