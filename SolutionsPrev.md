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

---

## 🔁 Replacement Block — Präsenzblatt 11 (with analogy + decision logic)

## 🗺️ Topic 4 (Revised): Floyd-Warshall Table Reduction + Dijkstra Counterexamples (Präsenzblatt 11)

### 🧠 Big-picture analogy: City Navigation Control Room
You are managing a city routing center.
- **Floyd-Warshall** = build a giant “all pairs shortest path” map.
- **Dijkstra** = one-source GPS that assumes roads never have “time refunds” (negative edges).

---

### 📚 Abbreviations
- **APSP** = All-Pairs Shortest Paths
- **SSSP** = Single-Source Shortest Paths
- **FW** = Floyd-Warshall
- **SPT** = Shortest-Path Tree

---

### 🧠 Aufgabe 1: Why one table is enough in Floyd-Warshall

#### What is asked?
Show why we can update in-place:
\[
d_{ij}=\min(d_{ij},d_{ik}+d_{kj})
\]
instead of keeping separate $D^{(k-1)}$ and $D^{(k)}$ tables.

---

#### Decision strategy
Key question:
> During iteration $k$, can $d_{ik}$ or $d_{kj}$ get “accidentally improved” in a way that breaks correctness?

If no, in-place update is safe.

---

#### Why this works
In iteration $k$, allowed intermediate nodes are from $\{1,\dots,k\}$.
For entries ending/starting at $k$:
- $d_{ik}^{(k)} = d_{ik}^{(k-1)}$
- $d_{kj}^{(k)} = d_{kj}^{(k-1)}$

Reason: a path from $i$ to $k$ using $k$ as an intermediate would be circular/useless for improvement; same for $k$ to $j$.

So helper terms used in
\[
d_{ij}\leftarrow\min(d_{ij},d_{ik}+d_{kj})
\]
are stable during this $k$-round. Therefore overwriting is safe.

\[
\boxed{\text{One table is sufficient.}}
\]

---

### 🧠 Aufgabe 2: Construct edge weights for 3 Dijkstra scenarios

Given directed graph structure from the sheet (nodes $s,A,B,C,D$ with shown arrows).  
We provide valid weight assignments that satisfy each requested condition.

---

#### 2.1 One negative edge, Dijkstra fails

##### Decision
Force Dijkstra to “finalize too early”, then reveal a better path using one negative edge later.

Choose edges:
- $s\to A=2$
- $s\to D=5$
- $A\to B=2$
- $D\to B=-4$  (only negative edge)
- $B\to C=1$

True shortest to $B$:
- via $A$: $2+2=4$
- via $D$: $5+(-4)=1$ (better)

Dijkstra from $s$ may settle $A$ then $B=4$ before exploring $D$ path correctly (implementation/finalization effect), yielding wrong distances.

\[
\boxed{\text{Condition 1 satisfied (single negative edge can break Dijkstra).}}
\]

---

#### 2.2 All edge weights negative, but Dijkstra still correct

##### Decision
Make graph such that from $s$ every node has only one reachable route (no competing alternatives to revise).

Example:
- $s\to A=-1$
- $A\to B=-1$
- $B\to C=-1$
- (other existing arrows can be set so they are unreachable from $s$ or absent in directed sense for path competition)

If each reachable node has exactly one path from $s$, Dijkstra cannot choose wrong among alternatives.

\[
\boxed{\text{All negative edges and still correct is possible in this constrained structure.}}
\]

---

#### 2.3 Multiple different shortest-path trees from $s$

##### Decision
Create ties: two distinct predecessors give equal shortest distance.

Example:
- $s\to A=1$
- $s\to D=1$
- $A\to B=1$
- $D\to B=1$
- $B\to C=1$

Distance to $B$ is 2 via either $A$ or $D$, so two distinct SPTs exist (different parent of $B$), same distances.

\[
\boxed{\text{Condition 3 satisfied (different SPTs with equal distances).}}
\]

---

### ⚠️ Exam traps
1. Saying “Dijkstra always fails with negatives” (not always — only generally unsafe).
2. Forgetting to justify *why* your weight assignment enforces each condition.
3. Confusing “same distances” with “same tree” in part 3.

---

### 🧩 Micro-summary
- Floyd-Warshall can be done in-place safely.
- Dijkstra fails with suitable negative-edge constructions.
- Negative edges do not automatically imply failure if no competing revisions exist.
- Equal-distance ties produce different shortest-path trees.

---

## 🔁 Replacement Block — Präsenzblatt 10 (with analogy + decision logic)

## 🎨 Topic 5 (Revised): Huffman Coding + Induction on Degree-2 Graph Components (Präsenzblatt 10)

### 🧠 Big-picture analogy
- **Huffman**: packing common words into shorter shorthand to save message length.
- **Graph induction**: people can hold at most two hands, so only line/circle/isolated formations are possible.

---

### 📚 Abbreviations
- **Huffman coding**: optimal prefix-free variable-length code.
- **Prefix-free**: no codeword is prefix of another.
- **deg(v)**: degree of node $v$.
- **ZHK**: connected component.

---

### 🧠 Aufgabe 1: Huffman coding for “ESGIBTFREIBIER”

#### Step 1: Frequency count (decision: always start here)
Word: E S G I B T F R E I B I E R

Counts:
- E: 3
- I: 3
- B: 2
- R: 2
- S: 1
- G: 1
- T: 1
- F: 1

#### Why this step matters
Huffman is greedy by frequency: least frequent symbols merge first.

---

#### Step 2: Build Huffman tree (decision: repeatedly merge two smallest)
One valid merge sequence:
1. S(1)+G(1)=2
2. T(1)+F(1)=2
3. (SG)(2)+B(2)=4
4. R(2)+(TF)(2)=4
5. E(3)+I(3)=6
6. (SGB)(4)+(RTF)(4)=8
7. 6 + 8 = 14 (root)

Assign bits (left=0, right=1) consistently.  
One valid code set:
- E: 00
- I: 01
- B: 101
- R: 100
- S: 1100
- G: 1101
- T: 1110
- F: 1111

(Equivalent Huffman trees/codes with same total length are also valid.)

---

#### Step 3: Total bit count
- E and I: $6$ chars × $2$ bits = $12$
- B and R: $4$ chars × $3$ bits = $12$
- S,G,T,F: $4$ chars × $4$ bits = $16$

Total:
\[
12+12+16=\boxed{40\text{ bits}}
\]

---

#### Why this is optimal
Huffman theorem: greedy merge of two smallest frequencies is optimal for prefix-free coding.

---

### ⚠️ Exam traps (Huffman)
1. Wrong frequencies from the word.
2. Non-prefix-free code assignment.
3. Assuming only one unique optimal code (often multiple exist).

---

### 🧠 Aufgabe 2: If all degrees $\le 2$, each component is isolated node / simple path / simple cycle

Given finite undirected loop-free graph $G=(V,E)$ with $\deg(v)\le2$ for all $v$.

---

#### Decision strategy for proof
Use induction on number of vertices in a connected component, and exploit the degree cap:
- no node can branch to 3 directions,
- so “tree-like branching” is impossible.

---

#### Proof sketch (clean exam style)

Base cases:
- 1 node: isolated node.
- 2 nodes connected: simple path.
- 3-cycle possible: simple cycle.

Induction step on connected component $Z$:
- If some vertex has degree 1, follow chain through degree-2 interior nodes; cannot branch (degree cap), so structure is a simple path.
- If all vertices in $Z$ have degree 2, finite connected undirected graph with all degree 2 must form one simple cycle.
- If degree 0 and connected, single isolated node.

Thus each connected component is exactly one of:
1. isolated node,
2. simple path (at least 2 nodes),
3. simple cycle (at least 3 nodes).

\[
\boxed{\text{Statement proven.}}
\]

---

### Analogy tie-in
People at a party can hold at most two hands:
- holds no hand → alone (isolated),
- chain of people → path,
- everyone in closed ring → cycle.
No “Y-shaped” structure possible because that needs someone holding 3 hands.

---

### ⚠️ Exam traps
1. Forgetting connected-component perspective.
2. Missing “all degree 2 in finite connected graph implies cycle”.
3. Accidentally allowing branching nodes.

---

### 🧩 Micro-summary
- Huffman result for the word: **40 bits**.
- Degree $\le2$ components are exactly isolated/path/cycle.

---

### 🧩 Micro-summary
- Kruskal/Prim both give MST weight 22.
- Components in subgraph $H$ via filtered BFS/DFS are linear-time.

---
---

## 🔁 Replacement Block — Präsenzblatt 9 (with analogy + decision logic)

## 🧱 Topic 6 (Revised): Matrix Chain Multiplication + Stable Cake DP (Präsenzblatt 9)

### 🧠 Big-picture analogies
- **Matrix chain**: You are assembling furniture panels. Same final product, but assembly order can massively change effort.
- **Cake stacking**: You build a tiered cake with strict size order (big at bottom, smaller on top), like legal stacking rules in a game.

---

### 📚 Abbreviations
- **MCM** = Matrix Chain Multiplication
- **DP** = Dynamic Programming
- **Optimal substructure**: optimal global solution built from optimal subsolutions
- **State**: what your DP table entry represents

---

### 🧠 Aufgabe 1: Optimal parenthesization for
\[
A_1(35\times15),\ A_2(15\times5),\ A_3(5\times10),\ A_4(10\times20)
\]

---

#### Decision strategy
For matrix-chain problems:
1. Define dimension vector $p$ with $A_i$ of size $p_{i-1}\times p_i$.
2. Use DP table $m[i,j]$ = minimal scalar multiplications for chain $A_i\cdots A_j$.
3. Try all split points $k$:
\[
m[i,j]=\min_{i\le k<j}\big(m[i,k]+m[k+1,j]+p_{i-1}p_kp_j\big)
\]

This is the standard professor-safe method (no guessing).

---

#### Step 1: Dimensions
\[
p=[35,15,5,10,20]
\]

#### Step 2: Length-2 chains
- $m[1,2]=35\cdot15\cdot5=2625$
- $m[2,3]=15\cdot5\cdot10=750$
- $m[3,4]=5\cdot10\cdot20=1000$

#### Step 3: Length-3 chains
\[
m[1,3]=\min\{
(m[1,1]+m[2,3]+35\cdot15\cdot10),\
(m[1,2]+m[3,3]+35\cdot5\cdot10)\}
\]
- split at 1: $0+750+5250=6000$
- split at 2: $2625+0+1750=4375$
So:
\[
m[1,3]=4375
\]

\[
m[2,4]=\min\{
(m[2,2]+m[3,4]+15\cdot5\cdot20),\
(m[2,3]+m[4,4]+15\cdot10\cdot20)\}
\]
- split at 2: $0+1000+1500=2500$
- split at 3: $750+0+3000=3750$
So:
\[
m[2,4]=2500
\]

#### Step 4: Length-4 chain
\[
m[1,4]=\min\{
m[1,1]+m[2,4]+35\cdot15\cdot20,\
m[1,2]+m[3,4]+35\cdot5\cdot20,\
m[1,3]+m[4,4]+35\cdot10\cdot20
\}
\]
- split at 1: $0+2500+10500=13000$
- split at 2: $2625+1000+3500=7125$
- split at 3: $4375+0+7000=11375$

Minimum:
\[
\boxed{m[1,4]=7125}
\]
Best split is at $k=2$, so parenthesization:
\[
\boxed{(A_1A_2)(A_3A_4)}
\]

---

#### Why this works
Matrix multiplication is associative, so final matrix same regardless of brackets; only operation count changes. DP is valid because each split creates independent subchains.

---

### ⚠️ Exam traps (MCM)
1. Using wrong dimensions in cost term.
2. Forgetting to compare all split points.
3. Guessing parentheses without DP evidence.

---

### 🧩 Micro-summary (Aufgabe 1)
- Optimal parenthesization: $\boxed{(A_1A_2)(A_3A_4)}$
- Minimal scalar multiplications: $\boxed{7125}$

---

### 🍰 Aufgabe 2: “Tatsächlich tragfähige Teiltortentürme” (descending layer sizes)

#### What is asked?
Modify old counting approach so only representations with **non-increasing layer sizes** are allowed (larger/equal below, smaller/equal above — here with distinct forms usually strictly descending by chosen form sequence logic).

---

#### Why old 1D DP is not enough
Old recurrence
\[
T(n)=\sum_{v\in V}T(n-v)
\]
counts different orders separately, so it overcounts and allows invalid orderings.

You must encode order constraints in the state.

---

#### Decision strategy (key)
Sort springform sizes:
\[
v_1 < v_2 < \dots < v_m
\]
Define DP:
\[
DP[i][s] = \text{number of ways to build size } s \text{ using only } \{v_1,\dots,v_i\}
\]
This automatically enforces a canonical order and prevents counting illegal permutations.

Equivalent interpretation:
- choose from small set first (combinatorial coin-change style),
- each combination corresponds to one descending physical stack order.

---

#### Recurrence
Base:
- $DP[i][0]=1$ for all $i$ (empty remainder)
- $DP[0][s>0]=0$

Transition:
\[
DP[i][s]=DP[i-1][s] + DP[i][s-v_i]\quad \text{if } s\ge v_i
\]
Else:
\[
DP[i][s]=DP[i-1][s]
\]

Why:
- first term: do not use size $v_i$
- second term: use at least one $v_i$ (and may use it again if allowed by problem model)

If each springform can be used at most once, replace second term with $DP[i-1][s-v_i]$.

---

#### Correctness intuition
By restricting allowed set to first $i$ sizes, each composition is generated in one canonical order only, so no permutation overcount. This enforces the structural sorting condition required by the exercise.

---

#### Complexity
Table size: $m\times n$ where $m=|V|$.
Each entry $O(1)$:
\[
\boxed{O(mn)}
\]
Space $O(mn)$ (or $O(n)$ with rolling optimization).

---

### ⚠️ Exam traps (cake DP)
1. Reusing old 1D recurrence unchanged.
2. Not encoding order in state.
3. Confusing “distinct forms” with “distinct usage count” rule.

---

### 🧩 Micro-summary (Aufgabe 2)
- Add second DP dimension to encode allowed form set.
- This prevents overcounting by permutations.
- Runtime polynomial: $O(|V|\,n)$.

---

## 🔁 Replacement Block — Präsenzblatt 8 (with analogy + decision logic)

## 🌳 Topic 7 (Revised): BST/AVL Checks + AVL Insertions + Intro Cake Counting (Präsenzblatt 8)

### 🧠 Big-picture analogies
- **BST**: library shelves sorted by title — left smaller, right larger.
- **AVL**: the same library, but shelves must also stay height-balanced to avoid long walks.
- **Rotations**: rebalancing furniture without changing sorted order.

---

### 📚 Abbreviations
- **BST** = Binary Search Tree
- **AVL tree** = self-balancing BST with height difference at most 1 per node
- **Balance factor** = height(left) - height(right)
- **LL/LR/RR/RL** = rotation cases (left-left etc.)

---

### 🧠 Aufgabe 1a: Check two given trees for BST property and AVL condition

#### Decision checklist
For each tree:
1. Verify BST ordering globally (not only local parent-child checks).
2. Compute subtree heights and check $|h_L-h_R|\le1$ at every node.

---

#### Tree 1 (left drawing in sheet)
Structure (from sheet): root 4; left 1 with children 0,2; right 5 with children 3,7 and 7 has children 6,8.

- **BST?** No.  
  Reason: node 3 is in right subtree of 4 but $3<4$ → violates global BST rule.
- **AVL?** Yes (height differences at all nodes are within 1).

\[
\boxed{\text{Tree 1: BST = No,\ AVL = Yes}}
\]

---

#### Tree 2 (right drawing in sheet)
Structure: root 4; left 2 with children 1 and 3, and 1 has child 0; right 6.

- **BST?** Yes (all left values < parent < all right values globally).
- **AVL?** Yes (balance factors within $\{-1,0,1\}$ everywhere).

\[
\boxed{\text{Tree 2: BST = Yes,\ AVL = Yes}}
\]

---

### ⚠️ Exam traps (1a)
1. Checking only parent-child inequality (local) instead of subtree range (global BST rule).
2. Forgetting to compute AVL condition at every node.

---

### 🧠 Aufgabe 1b: Insert sequence into AVL
Insert:
\[
1,\ 0,\ 5,\ 4,\ 3,\ 2,\ 6
\]

#### Decision strategy
After each insert:
1. Do normal BST insert.
2. Walk back up to first unbalanced node.
3. Identify case (LL/LR/RR/RL).
4. Rotate minimally to restore AVL.

---

#### Step trace with justification
1. Insert 1 → root.
2. Insert 0 → left of 1. Balanced.
3. Insert 5 → right of 1. Balanced.
4. Insert 4 → left of 5. Still balanced.
5. Insert 3 → left of 4 causes node 5 unbalanced (LL at 5).  
   **Action:** right rotation at 5.
6. Insert 2 → goes under left side, now node 1 becomes right-heavy via right subtree’s left path (RL/LR context depending representation).  
   Equivalent repair leads to root 4 after proper rotations.
7. Insert 6 → right side of 5, balanced.

Final AVL (one valid final shape):
- root 4
- left subtree rooted at 2 with children 1 and 3, and 1 has child 0
- right subtree rooted at 5 with child 6

---

#### Why rotations work
Rotations preserve in-order sequence (so BST property remains) while reducing local height imbalance.

---

### 🧩 Micro-summary (Aufgabe 1)
- Tree checks: (No/Yes) and (Yes/Yes) respectively.
- AVL insertion uses local rotations to preserve global efficiency $O(\log n)$.

---

### 🍰 Aufgabe 2: Theoretical Cake Towers (n=5 and counting algorithm)

Given available forms $\{1,2,5\}$.

#### (a) All representations for $n=5$
If order matters (as in original simpler model):
- $1+1+1+1+1$
- permutations of $1+1+1+2$ (4 ways)
- permutations of $1+2+2$ (3 ways)
- $5$
Total:
\[
1+4+3+1=\boxed{9}
\]

#### (b) Counting algorithm (simple model)
Use recurrence:
\[
T(n)=T(n-1)+T(n-2)+T(n-5)
\]
with:
- $T(0)=1$
- $T(v<0)=0$

Bottom-up DP computes all values up to $n$ in:
\[
\boxed{O(n\cdot|V|)}
\]

---

### Analogy tie-in
This is like climbing stairs where allowed step sizes are 1,2,5. Number of ways to reach stair $n$ = sum of ways to reach predecessor stairs compatible with one last step.

---

### ⚠️ Exam traps
1. Mixing “order matters” and “order not mattering” models.
2. Forgetting base case $T(0)=1$.
3. Not handling negative indices as 0.

---

### 🧩 Micro-summary
- For $n=5$ and $\{1,2,5\}$ in the simple model: 9 ways.
- DP recurrence solves it efficiently.

---
