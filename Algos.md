# Algorithms and Data Structures (Cross-Checked from Notes)

This file consolidates algorithms and data structures found in:
- `DSEA_notes.md`
- `SolutionsAltklausuren.md`
- `SolutionsHA.md`
- `SolutionsPrev.md`
- `StudyGuide.md`

For each item: best use case, short steps, Python-style pseudocode, and complexity with derivation idea.

Pseudocode note: many snippets intentionally use helper placeholders (e.g., `augment`, `verify`, `bucket_by_digit`, `cost`, `feasible_starts`) to keep each algorithm short and exam-focused; helper names indicate exactly what operation must be implemented.

---

## A) Data Structures

### 1) Binary Heap (Min-Heap / Max-Heap)
**Best suited:** Fast repeated min/max extraction (priority queues, Dijkstra/Prim/Huffman).
**Step-by-step walkthrough:**
1. **Start:** Store as complete binary tree in array.
2. **Then:** Insert at end + sift-up.
3. **Next:** Extract root + swap with last + sift-down.
**Pseudocode:**
```python
# helpers: sift_up(heap, i), sift_down(heap, i)
def push(h, x):
    h.append(x); sift_up(h, len(h)-1)

def pop_min(h):
    h[0], h[-1] = h[-1], h[0]
    x = h.pop(); sift_down(h, 0); return x
```
**Time:** `push/pop = O(log n)` (move along tree height), `build_heap = O(n)` (sum of node heights).

### 2) Priority Queue
**Best suited:** Always process next most urgent/min-cost element.
**Step-by-step walkthrough:**
1. **Start:** Backed by heap/buckets.
2. **Then:** Push, decrease-key (if supported), pop-min/pop-max.
**Pseudocode:**
```python
pq = []
push(pq, (prio, item))
prio, item = pop_min(pq)
```
**Time:** Heap implementation: `O(log n)` updates/extracts.

### 3) Bucket Queue
**Best suited:** Integer priorities in bounded range `[0..k]` (e.g., bounded-weight MST/SP variants).
**Step-by-step walkthrough:**
1. **Start:** Array of buckets by key.
2. **Then:** Maintain current non-empty bucket pointer.
**Pseudocode:**
```python
buckets = [list() for _ in range(k+1)]
cur = 0
def push(x, key): buckets[key].append(x)
def pop_min():
    while not buckets[cur]: cur += 1
    return buckets[cur].pop()
```
**Time:** Often `O(1)` amortized per op, total near `O(k + m)` scan + inserts/extracts.

### 4) Dynamic Array
**Best suited:** Random-access array with amortized `O(1)` append.
**Step-by-step walkthrough:**
1. **Start:** On full capacity, allocate larger array and copy.
2. **Then:** Optionally shrink when sparse.
**Pseudocode:**
```python
def append(x):
    if n == cap: resize(2*cap)
    a[n] = x; n += 1
```
**Time:** Append amortized `O(1)` via geometric resizing; occasional resize costs spread over many appends.

### 5) Stack
**Best suited:** LIFO processing (DFS, parsing, undo, monotonic tricks).
**Step-by-step walkthrough:**
1. **Start:** Push/pop at one end.
**Pseudocode:**
```python
st.append(x)
x = st.pop()
```
**Time:** `O(1)` per operation.

### 6) Queue
**Best suited:** FIFO processing (BFS, scheduling).
**Step-by-step walkthrough:**
1. **Start:** Enqueue at tail, dequeue at head.
**Pseudocode:**
```python
q.append(x)
x = q.popleft()
```
**Time:** `O(1)` with deque/ring buffer.

### 7) Hash Table / Hash Map
**Best suited:** Expected `O(1)` insert/find/delete.
**Step-by-step walkthrough:**
1. **Start:** Hash key -> bucket/slot.
2. **Then:** Resolve collisions.
**Pseudocode:**
```python
def put(k, v):
    i = h(k) % m
    place_or_resolve(i, k, v)
```
**Time:** Expected `O(1)` under uniform hashing and bounded load factor.

### 8) Open Addressing (Linear Probing / Double Hashing)
**Best suited:** Hashing without pointer chains.
**Step-by-step intuition:**

1. **What is Linear Probing? (The "Mailbox Walk")**
   - Imagine slots `0..m-1` as a row of mailboxes.
   - Your key `k` wants to go to `h'(k)`.
   - If that slot is full, you walk one step right (wrapping to slot `0` at the array end).
   - If that one is full too, keep walking right.
   - At the end, wrap around to slot `0`.

2. **The exam formula**
   - `h(k, i) = (h'(k) + i) mod m`
   - `i = 0,1,2,...` is your probe attempt number.
   - Try `i=0` first; if blocked, try `i=1`, then `i=2`, etc.

3. **Search and deletion (the Tombstone trick)**
   - Search must follow the **same probe path** until:
     - key found, or
     - you hit an actually empty slot (`EMPTY`), which means "stop, not found".
   - If you delete by writing `EMPTY`, you can accidentally break later searches.
   - So deletion writes a marker like `DELETED` (tombstone):
     - searches skip over tombstones,
     - insertions may reuse tombstones.

4. **Double Hashing variant (same framework, better spread)**
   - Instead of always `+1`, jump by `h2(k)`.
   - Probe formula: `h(k,i) = (h1(k) + i*h2(k)) mod m`.
   - This reduces primary clustering vs linear probing.

5. **What exam tasks usually test**
   - Manual insertion trace with collisions and wrap-around.
   - Correct search path after deletions.
   - Why linear probing clusters and double hashing helps.
**Pseudocode:**
```python
# table is array of slots; EMPTY is unused, DELETED is tombstone
# slot_key(s) returns key stored in slot s (or EMPTY)
def find_slot(k):
    i = h1(k) % m
    step = 1              # linear probing; for double hashing use step = h2(k)
    while table[i] != EMPTY and (table[i] == DELETED or slot_key(table[i]) != k):
        i = (i + step) % m
    return i
```
**Time:** Insert/search expected `O(1)` at low/moderate load; both degrade sharply as load factor approaches 1.

### 9) Cuckoo Hashing
**Best suited:** Worst-case `O(1)` lookup with two hash locations.
**Step-by-step walkthrough:**
1. **Start:** Insert by evicting occupant to its alternate position.
2. **Then:** Rebuild on long cycles.
**Pseudocode:**
```python
# inA toggles table side; hA/hB are the two hash functions
def insert(current):
    for _ in range(limit):
        pos = hA(current) if inA else hB(current)
        evicted, table[pos] = table[pos], current
        if evicted is EMPTY: return
        current = evicted
    rehash()  # loop exhausted without EMPTY slot -> treat as cycle and rebuild
```
**Time:** Lookup `O(1)` worst-case; insert expected `O(1)` (rehash rare amortized).

### 10) Universal / 2-Universal Hash Family
**Best suited:** Provable low collision probability in randomized algorithms.
**Step-by-step walkthrough:**
1. **Start:** Randomly choose hash function from family.
2. **Then:** Use in hash table/minhash etc.
**Pseudocode:**
```python
# choose random a,b; p is prime >= universe size
# m is table size; 2-universal guarantee requires suitable family/params
a, b = random_uniform_params()
h(x) = ((a*x + b) % p) % m
```
**Time:** Hash eval `O(1)`; guarantee: `Pr[h(x)=h(y)] <= 1/m` (2-universal).

### 11) Binary Search Tree (BST)
**Best suited:** Ordered set/map with predecessor/successor queries.
**Step-by-step walkthrough:**
1. **Start:** Left < node < right recursively.
**Pseudocode:**
```python
def search(t, x):
    if not t or t.key == x: return t
    return search(t.left, x) if x < t.key else search(t.right, x)
```
**Time:** `O(h)`, worst `O(n)` if unbalanced.

### 12) AVL Tree
**Best suited:** Guaranteed logarithmic ordered operations.
**Step-by-step walkthrough:**
1. **Start:** BST insert/delete + update heights + rotations (LL/RR/LR/RL).
**Pseudocode:**
```python
def insert(t, x):
    t = bst_insert(t, x)
    update_height(t)
    return rebalance(t)
```
**Time:** `O(log n)` operations since AVL keeps height `h = O(log n)`.

### 13) Union-Find (Disjoint Set Union)
**Best suited:** Dynamic connectivity (Kruskal, components, cycle checks).
**Step-by-step walkthrough:**
1. **Start:** `find` root with path compression.
2. **Then:** `union` by rank/size.
**Pseudocode:**
```python
def find(x):
    if p[x] != x: p[x] = find(p[x])
    return p[x]

def union(a,b):
    ra, rb = find(a), find(b)
    if ra != rb: link_by_rank(ra, rb)  # attach smaller-rank tree to larger
```
**Time:** Amortized `O(alpha(n))` per op (inverse Ackermann).

### 14) Residual Network (flow DS)
**Best suited:** Represent current flow + possible augment/reverse moves.
**Step-by-step walkthrough:**
1. **Start:** Maintain residual capacities for forward/backward edges.
**Pseudocode:**
```python
res[u][v] = cap[u][v] - flow[u][v]
res[v][u] = flow[u][v]
```
**Time:** Structure update `O(path length)` after augmentation.

### 15) Prefix-Free Code Tree (Huffman/Shannon-Fano output structure)
**Best suited:** Decodable variable-length coding.
**Step-by-step walkthrough:**
1. **Start:** Build binary tree.
2. **Then:** Leaf path bits are codewords.
**Pseudocode:**
```python
def decode(bits, root):
    u = root
    out = []
    for b in bits:
        u = u.left if b == 0 else u.right
        if u.is_leaf:
            out.append(u.symbol)
            u = root
    return out
```
**Time:** Decode `O(number_of_bits)`.

---

## B) Core Graph / Flow Algorithms

### 16) BFS (Breadth-First Search)
**Best suited:** Unweighted shortest paths, layering, bipartite test.
**Step-by-step walkthrough:**
1. **Start:** Queue from source.
2. **Then:** Visit neighbors level by level.
**Pseudocode:**
```python
q = deque([s]); dist[s] = 0
while q:
    u = q.popleft()
    for v in G[u]:
        if v not in dist: dist[v] = dist[u] + 1; q.append(v)
```
**Time:** `O(V+E)` (each vertex/edge processed once).

### 17) DFS (Depth-First Search)
**Best suited:** Reachability, components, cycle detection, ordering.
**Step-by-step walkthrough:**
1. **Start:** Recurse/stack deeply then backtrack.
**Pseudocode:**
```python
def dfs(u):
    vis.add(u)
    for v in G[u]:
        if v not in vis: dfs(v)
```
**Time:** `O(V+E)`.

### 18) Connected Components
**Best suited:** Partition undirected graph into maximal connected subgraphs.
**Step-by-step walkthrough:**
1. **Start:** Run BFS/DFS from each unvisited node, assign component id.
**Pseudocode:**
```python
cid = 0
for u in V:
    if u not in vis: bfs_label(u, cid); cid += 1
```
**Time:** `O(V+E)`.

### 19) Dijkstra
**Best suited:** Single-source shortest paths with nonnegative edges.
**Step-by-step walkthrough:**
1. **Start:** Dist init.
2. **Then:** Repeatedly settle min-distance node.
3. **Next:** Relax outgoing edges.
**Pseudocode:**
```python
dist = {s: 0}; pq = [(0, s)]
while pq:
    d, u = heappop(pq)
    if d != dist[u]: continue
    for v, w in G[u]:
        nd = d + w
        if nd < dist.get(v, INF): dist[v] = nd; heappush(pq, (nd, v))
```
**Time:** `O((V+E) log V)` with heap; each relax causes heap op.

### 20) Bellman-Ford
**Best suited:** Shortest paths with negative edges + negative-cycle detection.
**Step-by-step walkthrough:**
1. **Start:** Relax all edges `V-1` rounds.
2. **Then:** One extra round checks cycle.
**Pseudocode:**
```python
dist[s] = 0
for _ in range(V-1):
    for u, v, w in E: dist[v] = min(dist[v], dist[u] + w)
neg_cycle = any(dist[u] + w < dist[v] for u, v, w in E)
```
**Time:** `O(VE)` from nested rounds × edges.

### 21) Floyd-Warshall
**Best suited:** All-pairs shortest paths (dense graphs, small/medium V).
**Step-by-step walkthrough:**
1. **Start:** DP over allowed intermediate vertices `k`.
**Pseudocode:**
```python
for k in V:
    for i in V:
        for j in V:
            d[i][j] = min(d[i][j], d[i][k] + d[k][j])
```
**Time:** `O(V^3)` (three loops).

### 22) Kruskal MST
**Best suited:** Sparse MST construction.
**Step-by-step walkthrough:**
1. **Start:** Sort edges by weight.
2. **Then:** Add if endpoints in different DSU sets.
**Pseudocode:**
```python
E.sort(key=lambda e: e.w)
for e in E:
    if find(e.u) != find(e.v): union(e.u, e.v); mst.append(e)
```
**Time:** `O(E log E)` sorting dominates; DSU almost constant amortized.

### 23) Prim MST
**Best suited:** MST by growing one tree from any start node.
**Step-by-step walkthrough:**
1. **Start:** Keep min crossing edge via priority queue.
**Pseudocode:**
```python
vis = {s}; push all edges from s
while pq and len(vis) < V:
    w, u, v = heappop(pq)
    if v in vis: continue
    vis.add(v); mst.append((u, v, w)); push edges of v
```
**Time:** `O((V+E) log V)` with heap.

### 24) Borůvka MST
**Best suited:** Parallel-friendly MST; components shrink quickly.
**Step-by-step walkthrough:**
1. **Start:** Each component picks cheapest outgoing edge.
2. **Then:** Merge.
3. **Next:** Repeat.
**Pseudocode:**
```python
while num_components > 1:
    cheapest = pick_cheapest_outgoing_per_component()
    for e in cheapest: if find(e.u) != find(e.v): union(e.u, e.v)
```
**Time:** `O(E log V)`; components at least halve each round.

### 25) Topological Sort (Kahn)
**Best suited:** Linearization of DAG dependencies.
**Step-by-step walkthrough:**
1. **Start:** Compute indegrees.
2. **Then:** Queue zero-indegree nodes.
3. **Next:** Remove iteratively.
**Pseudocode:**
```python
q = deque([u for u in V if indeg[u] == 0])
while q:
    u = q.popleft(); order.append(u)
    for v in G[u]:
        indeg[v] -= 1
        if indeg[v] == 0: q.append(v)
```
**Time:** `O(V+E)`.

### 26) Ford-Fulkerson (augmenting paths)
**Best suited:** Max flow foundation.
**Step-by-step walkthrough:**
1. **Start:** Repeatedly find augmenting path in residual graph, augment by bottleneck.
**Pseudocode:**
```python
flow = 0
while path := find_path_in_residual(s, t):
    b = min(res[e] for e in path)
    augment(path, b); flow += b
```
**Time:** `O(E * |f*|)` for integer capacities with generic path choice.

### 27) Edmonds-Karp
**Best suited:** Deterministic polynomial max flow (BFS augmenting path).
**Step-by-step walkthrough:**
1. **Start:** Ford-Fulkerson with shortest augmenting path in edge count.
**Pseudocode:**
```python
while path := bfs_residual_path(s, t):
    b = bottleneck(path); augment(path, b)
```
**Time:** `O(VE^2)` (each BFS `O(E)`, critical-edge argument bounds augmentations).

### 28) Cycle-Canceling in Flows
**Best suited:** Convert arbitrary feasible flow to acyclic flow / reduce circulation cost cases.
**Step-by-step walkthrough:**
1. **Start:** Detect positive-flow directed cycle.
2. **Then:** Push minimum cycle flow backward.
3. **Next:** Repeat.
**Pseudocode:**
```python
while cycle := find_positive_flow_cycle():
    d = min(flow[e] for e in cycle)
    for e in cycle: flow[e] -= d
```
**Time:** Polynomial with proper cycle detection; each cancel removes at least one positive edge on that cycle.

### 29) Bipartite Matching via Max Flow
**Best suited:** Maximum cardinality matching in bipartite graphs.
**Step-by-step walkthrough:**
1. **Start:** Source -> left (cap1), left->right edges (cap1), right->sink (cap1), run max flow.
**Pseudocode:**
```python
build_unit_capacity_network(L, R, E)
f = max_flow(s, t)
matching = {(u, v) for (u, v) in E if flow[u][v] == 1}
```
**Time:** Depends on flow solver (e.g., Edmonds-Karp `O(VE^2)`).

---

## C) Sorting, Selection, and Search

### 30) Merge Sort
**Best suited:** Stable `O(n log n)` sorting.
**Step-by-step walkthrough:**
1. **Start:** Divide in halves, recursively sort, merge two sorted halves.
**Pseudocode:**
```python
def mergesort(a):
    if len(a) <= 1: return a
    m = len(a)//2
    return merge(mergesort(a[:m]), mergesort(a[m:]))
```
**Time:** `T(n)=2T(n/2)+O(n)` => `O(n log n)` (Master theorem).

### 31) QuickSort (randomized)
**Best suited:** Fast in-place average sorting.
**Step-by-step walkthrough:**
1. **Start:** Pick pivot, partition, recurse on sides.
**Pseudocode:**
```python
def quicksort(a, l, r):
    if l >= r: return
    p = partition(a, l, r)
    quicksort(a, l, p-1); quicksort(a, p+1, r)
```
**Time:** Expected `O(n log n)`, worst `O(n^2)`; expectation from pivot rank probabilities.

### 32) QuickSelect
**Best suited:** k-th smallest element.
**Step-by-step walkthrough:**
1. **Start:** Partition by pivot.
2. **Then:** Recurse only side containing k.
**Pseudocode:**
```python
def quickselect(a, k):
    p = partition(a)
    if k == p: return a[p]
    return quickselect(left_or_right_part, adjusted_k)
```
**Time:** Expected `O(n)` (`T(n)=T(expected fraction*n)+O(n)`).

### 33) Selection Sort
**Best suited:** Tiny arrays / teaching.
**Step-by-step walkthrough:**
1. **Start:** Repeatedly select minimum of unsorted suffix and swap front.
**Pseudocode:**
```python
for i in range(n):
    j = i + argmin(a[i:])
    a[i], a[j] = a[j], a[i]
```
**Time:** `O(n^2)` comparisons (`n+(n-1)+...+1`).

### 34) Insertion Sort
**Best suited:** Nearly sorted arrays, small base cases.
**Step-by-step walkthrough:**
1. **Start:** Insert each element into sorted prefix.
**Pseudocode:**
```python
for i in range(1, n):
    x = a[i]; j = i-1
    while j >= 0 and a[j] > x: a[j+1] = a[j]; j -= 1
    a[j+1] = x
```
**Time:** Worst `O(n^2)`, best `O(n)` when already sorted.

### 35) Counting Sort
**Best suited:** Integer keys in small range.
**Step-by-step walkthrough:**
1. **Start:** Count frequencies, prefix sums, place stably.
**Pseudocode:**
```python
cnt = [0]*(k+1)
for x in a: cnt[x] += 1
for i in range(1, k+1): cnt[i] += cnt[i-1]
```
**Time:** `O(n+k)` (linear in elements + key range).

### 36) Radix Sort (incl. MSD Radix Sort)
**Best suited:** Fixed-length strings/integers with bounded alphabet/base.
**Step-by-step walkthrough:**
1. **Start:** Sort by digits/chars (LSD or recursive MSD buckets).
**Pseudocode:**
```python
def msd(arr, d):
    if len(arr) <= 1: return arr
    buckets = bucket_by_digit(arr, d)
    return concat(msd(b, d+1) for b in buckets)
```
**Time:** `O((n + b) * digits)` (or `O(n * digits)` with constant base).

### 37) 2/3 Sort (exam-specific divide-and-conquer)
**Best suited:** Special recurrence exercise.
**Step-by-step walkthrough:**
1. **Start:** Recursively sort overlapping/partial segments of size about `2n/3`.
**Pseudocode:**
```python
# exam-specific overlap D&C recurrence example
def two_thirds_sort(a, l, r):
    if r-l <= 1: return
    t = (r-l+1)//3
    two_thirds_sort(a, l, r-t)
    two_thirds_sort(a, l+t, r)
    two_thirds_sort(a, l, r-t)
```
**Time:** From its recurrence (typically superlinear; derive via master/recursion tree for given variant).

### 38) Binary Search
**Best suited:** Membership/position in sorted arrays.
**Step-by-step walkthrough:**
1. **Start:** Compare with midpoint.
2. **Then:** Discard half each iteration.
**Pseudocode:**
```python
l, r = 0, n-1
while l <= r:
    m = (l+r)//2
    if a[m] == x: return m
    if a[m] < x: l = m+1
    else: r = m-1
```
**Time:** `O(log n)` (search interval halves each step).

### 39) Sequential Search (Linear Search)
**Best suited:** Unsorted/small collections.
**Step-by-step walkthrough:**
1. **Start:** Scan left to right until match/end.
**Pseudocode:**
```python
for i, v in enumerate(a):
    if v == x: return i
return -1
```
**Time:** `O(n)` worst; expected `O(n)` without extra distribution assumptions.

### 40) Two-Pointer Technique
**Best suited:** Sorted-array pair/triplet constraints, window-like scans.
**Step-by-step walkthrough:**
1. **Start:** Maintain two indices and move based on condition.
**Pseudocode:**
```python
i, j = 0, n-1
while i < j:
    s = a[i] + a[j]
    if s == target: return True
    if s < target: i += 1
    else: j -= 1
```
**Time:** `O(n)` (each pointer moves at most n times).

### 41) Sliding Window
**Best suited:** Contiguous subarray/string constraints.
**Step-by-step walkthrough:**
1. **Start:** Expand right, shrink left while constraint violated.
**Pseudocode:**
```python
l = 0
for r in range(n):
    add(a[r])
    while bad(): remove(a[l]); l += 1
```
**Time:** `O(n)` amortized when each index enters/leaves window once.

### 42) Boyer-Moore Majority Vote
**Best suited:** Find majority element (`> n/2`) in linear time, constant space.
**Step-by-step walkthrough:**
1. **Start:** Cancel out different elements via counter.
**Pseudocode:**
```python
cand = None; c = 0
for x in a:
    if c == 0: cand, c = x, 1
    elif x == cand: c += 1
    else: c -= 1
```
**Time:** `O(n)`; cancellation invariant proves surviving candidate is majority if one exists.

### 43) Celebrity Problem (elimination)
**Best suited:** Find node known by everyone, knowing nobody.
**Step-by-step walkthrough:**
1. **Start:** Pairwise eliminate impossible candidates, then verify final candidate.
**Pseudocode:**
```python
c = 0
for i in range(1, n):
    if knows(c, i): c = i
verify(c)  # check candidate knows nobody and everybody knows candidate
```
**Time:** `O(n)` queries for elimination + `O(n)` verification.

---

## D) Divide-and-Conquer / Numeric

### 44) DFT (Discrete Fourier Transform)
**Best suited:** Transform polynomial coefficients <-> value form.
**Step-by-step walkthrough:**
1. **Start:** Evaluate polynomial at roots of unity.
**Pseudocode:**
```python
def dft(a, w):
    return [sum(a[j] * (w**(k*j)) for j in range(n)) for k in range(n)]
```
**Time:** Direct DFT `O(n^2)` (double summation).

### 45) FFT (Fast Fourier Transform, Cooley-Tukey)
**Best suited:** Fast polynomial multiplication and convolution.
**Step-by-step walkthrough:**
1. **Start:** Split even/odd coefficients recursively.
2. **Then:** Combine with twiddle factors.
**Pseudocode:**
```python
def fft(a):
    n = len(a)
    if n == 1: return a
    E = fft(a[0::2]); O = fft(a[1::2])
    return combine(E, O)
```
**Time:** `T(n)=2T(n/2)+O(n)` => `O(n log n)`.

### 46) Karatsuba Multiplication
**Best suited:** Large integer multiplication faster than grade-school.
**Step-by-step walkthrough:**
1. **Start:** Split numbers high/low.
2. **Then:** Do 3 recursive multiplies instead of 4.
**Pseudocode:**
```python
# small: base case, split: high/low halves, B=base^(half), B2=B*B
def kara(x, y):
    if small(x,y): return x*y
    a,b,c,d = split(x,y)
    ac = kara(a,c); bd = kara(b,d); s = kara(a+b, c+d)
    return ac*B2 + (s-ac-bd)*B + bd
```
**Time:** `T(n)=3T(n/2)+O(n)` => `O(n^{log2 3})`.

### 47) Strassen Matrix Multiplication
**Best suited:** Large dense matrix multiplication.
**Step-by-step walkthrough:**
1. **Start:** Block matrices.
2. **Then:** Compute 7 subproducts (not 8) + recombination.
**Pseudocode:**
```python
# high-level only: helpers encapsulate Strassen's M1..M7 formulas and C-block recombination
def strassen(A, B):
    if small: return naive_mul(A,B)
    A11,A12,A21,A22 = split_blocks(A); B11,B12,B21,B22 = split_blocks(B)
    M1, M2, M3, M4, M5, M6, M7 = compute_strassen_products(A11,A12,A21,A22,B11,B12,B21,B22)
    return combine_blocks_from_products(M1, M2, M3, M4, M5, M6, M7)
```
**Time:** `T(n)=7T(n/2)+O(n^2)` => `O(n^{log2 7})`.

---

## E) Dynamic Programming and Greedy

### 48) Matrix Chain Multiplication (DP)
**Best suited:** Optimal parenthesization of matrix product chain.
**Step-by-step walkthrough:**
1. **Start:** `dp[i][j] = min_k dp[i][k] + dp[k+1][j] + cost`.
**Pseudocode:**
```python
for len_ in range(2, n+1):
    for i in range(1, n-len_+2):
        j = i + len_ - 1
        dp[i][j] = min(dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j] for k in range(i, j))
```
**Time:** `O(n^3)` (length/start/split loops).

### 49) LCS (Longest Common Subsequence) DP
**Best suited:** Sequence similarity / edit-like tasks.
**Step-by-step walkthrough:**
1. **Start:** `dp[i][j]` from prefixes.
2. **Then:** Match => `+1`, else max of neighbors.
**Pseudocode:**
```python
if a[i-1] == b[j-1]: dp[i][j] = dp[i-1][j-1] + 1
else: dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```
**Time:** `O(nm)` table fill.

### 50) Hirschberg (space-optimized LCS reconstruction)
**Best suited:** Recover LCS string with linear extra memory.
**Step-by-step walkthrough:**
1. **Start:** Divide one string, run forward/backward DP rows, split optimally, recurse.
**Pseudocode:**
```python
def hirschberg(X, Y):
    if base_case: return direct
    mid = len(X)//2
    q = best_split_using_two_lcs_rows(X[:mid], X[mid:], Y)
    return hirschberg(X[:mid], Y[:q]) + hirschberg(X[mid:], Y[q:])
```
**Time:** `O(nm)` time, `O(min(n,m))` space.

### 51) Longest Palindromic Subsequence (LPS) DP
**Best suited:** Longest palindrome by deletion (subsequence, not substring).
**Step-by-step walkthrough:**
1. **Start:** Interval DP on `i..j`.
2. **Then:** Equal ends => `2+dp[i+1][j-1]` else max neighbors.
**Pseudocode:**
```python
for i in reversed(range(n)):
    dp[i][i] = 1
    for j in range(i+1, n):
        dp[i][j] = 2 + dp[i+1][j-1] if s[i]==s[j] else max(dp[i+1][j], dp[i][j-1])
```
**Time:** `O(n^2)` states/transitions.

### 52) 0/1 Knapsack DP
**Best suited:** Value maximization with indivisible items and capacity `W`.
**Step-by-step walkthrough:**
1. **Start:** Include/exclude transition on item index and remaining capacity.
**Pseudocode:**
```python
for i in range(1, n+1):
    for w in range(W+1):
        dp[i][w] = dp[i-1][w]
        if wt[i] <= w: dp[i][w] = max(dp[i][w], dp[i-1][w-wt[i]] + val[i])
```
**Time:** `O(nW)` pseudo-polynomial.

### 53) Fractional Knapsack (Greedy)
**Best suited:** Splittable items.
**Step-by-step walkthrough:**
1. **Start:** Sort by value/weight ratio descending.
2. **Then:** Take full then fraction.
**Pseudocode:**
```python
items.sort(key=lambda x: x.v/x.w, reverse=True)
for it in items:
    take = min(remW, it.w)
    ans += take * (it.v/it.w); remW -= take
```
**Time:** `O(n log n)` due to sorting.

### 54) Rod Cutting DP
**Best suited:** Max revenue by cutting rod into pieces.
**Step-by-step walkthrough:**
1. **Start:** `dp[len] = max(price[i] + dp[len-i])`.
**Pseudocode:**
```python
for L in range(1, n+1):
    dp[L] = max(price[i] + dp[L-i] for i in range(1, L+1))
```
**Time:** `O(n^2)`.

### 55) Coin Change
**Best suited:** Minimum coins / counting ways; also compare greedy vs DP correctness.
**Step-by-step walkthrough:**
1. **Start:** DP over amount.
2. **Then:** Transition by coin usage.
**Pseudocode:**
```python
dp = [INF]*(A+1); dp[0] = 0
for x in range(1, A+1):
    dp[x] = min((dp[x-c]+1 for c in coins if c<=x), default=INF)
```
**Time:** `O(A * #coins)`; greedy may fail on non-canonical systems.

### 56) Interval Scheduling (Greedy)
**Best suited:** Max number of non-overlapping intervals.
**Step-by-step walkthrough:**
1. **Start:** Sort by finish time.
2. **Then:** Repeatedly choose first compatible.
**Pseudocode:**
```python
intervals.sort(key=lambda it: it.end)
last = -INF
for it in intervals:
    if it.start >= last: pick(it); last = it.end
```
**Time:** `O(n log n)` sorting + linear scan.

### 57) Interval Coverage DP (Roman/resource coverage style)
**Best suited:** Cover line/requirements with minimal cost/choices under interval options.
**Step-by-step walkthrough:**
1. **Start:** Define `dp[pos]` = optimal value up to position.
2. **Then:** Transition by intervals ending at `pos`.
**Pseudocode:**
```python
# dp[p] = best cost to cover up to position p
# M = maximum position/endpoint to be covered
# transition checks all intervals [l..p] that can end at p
# helper computes min over such predecessor states + interval cost
for p in range(1, M+1):
    dp[p] = best_transition_from_intervals_covering(p)
```
**Time:** Depends on transition implementation (often `O(M + total_intervals)` or `O(M log M)`).

### 58) Bookshelf DP (exam variant)
**Best suited:** Partition/order optimization with shelf constraints.
**Step-by-step walkthrough:**
1. **Start:** `dp[i]` best for first `i` books.
2. **Then:** Try last shelf start `j`.
**Pseudocode:**
```python
# cost(j,i): cost of placing books j..i on one shelf
# feasible_starts(i): starts j satisfying width/constraint limits for shelf ending at i
# 1-indexed books: n = number of books, books are indexed 1..n; initialize dp[0] = 0
for i in range(1, n+1):
    dp[i] = min(cost(j,i) + dp[j-1] for j in feasible_starts(i))
```
**Time:** Typically `O(n^2)` unless optimized.

---

## F) Coding, Matching, Randomized / Specialized

### 59) Huffman Coding
**Best suited:** Optimal prefix-free coding for known symbol frequencies.
**Step-by-step walkthrough:**
1. **Start:** Repeatedly merge two lowest-frequency trees in min-heap.
**Pseudocode:**
```python
pq = [(freq[c], Leaf(c)) for c in alphabet]
heapify(pq)
while len(pq) > 1:
    f1, a = heappop(pq); f2, b = heappop(pq)
    heappush(pq, (f1+f2, Node(a, b)))
```
**Time:** `O(k log k)` for `k` symbols.

### 60) Shannon-Fano Coding
**Best suited:** Heuristic prefix coding (not always optimal like Huffman).
**Step-by-step walkthrough:**
1. **Start:** Sort by frequency.
2. **Then:** Recursively split into two near-equal frequency groups.
**Pseudocode:**
```python
def sf(symbols):
    if len(symbols) <= 1: return
    L, R = split_near_half_weight(symbols)
    prepend_bit_0(L); prepend_bit_1(R); sf(L); sf(R)
```
**Time:** Sorting dominates `O(k log k)`.

### 61) Gale-Shapley / Deferred Acceptance (Stable Matching)
**Best suited:** Stable matching (one-to-one or capacity variants).
**Step-by-step intuition:**

1. **The setup**
   - Split into two sides (e.g., proposers and acceptors).
   - Everyone has a strict preference list over the opposite side.
   - Initially everyone is free.

2. **Core loop**
   - Pick any free proposer who still has someone left to propose to.
   - They propose to their highest-ranked not-yet-asked acceptor.

3. **Acceptor decision**
   - If acceptor is free -> engage with the proposer.
   - If acceptor is already engaged:
     - compare current partner vs new proposer,
     - keep the preferred one,
     - reject the other (who becomes free).

4. **Why it ends**
   - No proposer asks the same acceptor twice.
   - There are at most `n^2` possible proposals.
   - So the loop must terminate.

5. **What you get at the end**
   - Everyone is matched.
   - No blocking pair exists (stable matching).
   - If proposers initiate, the result is proposer-optimal among stable matchings.
**Pseudocode:**
```python
# current[b] is b's current partner (or None)
# prefers[b][x] gives ranking score of proposer x for receiver b
# engage(a,b): pair a with b; free(x): mark proposer x as free
while free_proposer_exists():
    a = pick_free(); b = next_choice[a]
    if current[b] is None:
        engage(a,b)
    elif prefers[b][a] > prefers[b][current[b]]:
        old = current[b]
        engage(a,b); free(old)
```
**Time:** `O(n^2)` proposals upper bound (each pair proposed at most once).

### 62) MinHash
**Best suited:** Fast approximate Jaccard similarity at scale.
**Step-by-step walkthrough:**
1. **Start:** Apply random permutations/hash functions.
2. **Then:** Keep minimum hash per set.
3. **Next:** Compare signatures.
**Pseudocode:**
```python
def minhash_sig(S, hs):
    return [min(h(x) for x in S) for h in hs]
```
**Time:** `O(k*|S|)` for `k` hash functions; estimator variance decreases as `1/k`.

### 63) Jaccard Similarity Computation
**Best suited:** Exact set similarity baseline.
**Step-by-step walkthrough:**
1. **Start:** Compute intersection and union sizes.
**Pseudocode:**
```python
J = len(A & B) / len(A | B)
```
**Time:** Hash-set implementation `O(|A|+|B|)` expected.

### 64) Von Neumann Extractor
**Best suited:** Build fair coin from unknown biased coin (independent tosses).
**Step-by-step walkthrough:**
1. **Start:** Toss twice.
2. **Then:** HT->1, TH->0, HH/TT retry.
**Pseudocode:**
```python
def fair_bit():
    while True:
        a, b = biased(), biased()
        if (a,b)==('H','T'): return 1
        if (a,b)==('T','H'): return 0
```
**Time:** Expected pair-trials `= 1/(2p(1-p))`; effectively constant when `p` is bounded away from `0` and `1`.

### 65) Rejection Sampling (discrete)
**Best suited:** Simulate target distribution from easy base randomness.
**Step-by-step walkthrough:**
1. **Start:** Sample candidate uniformly from larger range.
2. **Then:** Reject out-of-range events.
**Pseudocode:**
```python
# generates a biased coin with Pr[1] = 1/n (1 iff accepted x equals 0)
def sample_bernoulli_1_n(n):
    k = ceil_log2(n)
    while True:
        x = rand_bits(k)
        if x < n: return 1 if x == 0 else 0
```
**Time:** Expected constant retries when acceptance probability is bounded away from 0.

---

## G) Self-Check Coverage Map (Source-Term -> Entry Above)

- Heaps / Min-Heap / Max-Heap / Heapify -> **1**
- Priority Queue -> **2**
- Bucket Queue -> **3**
- Dynamic Array -> **4**
- Stack / Queue -> **5, 6**
- Hash Table / Hash Map -> **7**
- Open Addressing / Linear Probing / Double Hashing -> **8**
- Cuckoo Hashing -> **9**
- Universal / 2-Universal Hashing -> **10**
- BST / AVL -> **11, 12**
- Union-Find / DSU -> **13**
- Residual Network -> **14**
- Prefix-Free Code structure -> **15**
- BFS / DFS / Connected Components -> **16, 17, 18**
- Dijkstra / Bellman-Ford / Floyd-Warshall -> **19, 20, 21**
- Kruskal / Prim / Borůvka -> **22, 23, 24**
- Topological Sort / DAG ordering -> **25**
- Max Flow / Ford-Fulkerson / Edmonds-Karp -> **26, 27**
- Cycle-Canceling flow -> **28**
- Bipartite Matching via Flow -> **29**
- MergeSort / QuickSort / QuickSelect -> **30, 31, 32**
- Selection / Insertion / Counting / Radix / MSD Radix / 2/3 Sort -> **33, 34, 35, 36, 37**
- Binary Search / Sequential Search -> **38, 39**
- Two-Pointer / Sliding Window -> **40, 41**
- Boyer-Moore Majority Vote / Celebrity Problem -> **42, 43**
- DFT / FFT -> **44, 45**
- Karatsuba / Strassen -> **46, 47**
- Matrix Chain / LCS / Hirschberg / LPS -> **48, 49, 50, 51**
- 0/1 Knapsack / Fractional Knapsack / Rod Cutting / Coin Change -> **52, 53, 54, 55**
- Interval Scheduling / Interval Coverage DP / Bookshelf DP -> **56, 57, 58**
- Huffman / Shannon-Fano -> **59, 60**
- Gale-Shapley / Deferred Acceptance / Stable Matching -> **61**
- MinHash / Jaccard -> **62, 63**
- Von Neumann extractor / Rejection sampling -> **64, 65**

---

## H) One-Evening Crash Order

If you only have one evening, do this exact order:

1. **Recurrences**
   - Practice case detection and one non-standard recurrence.
2. **Heap + Priority Queue mechanics**
   - Insert, extract-min, sift-up/down by hand.
3. **Dijkstra + shortest-path traps**
   - Relaxation loop, priority queue updates, and "no negative edges" rule.
4. **MST package: Kruskal + Prim + Union-Find**
   - Be able to trace both and explain when each is convenient.
5. **Hashing package**
   - Linear probing, tombstones, double hashing, cuckoo high-level idea.
6. **AVL essentials**
   - Insert/delete and identify LL, RR, LR, RL rotations.
7. **Dynamic Programming core pattern**
   - State, transition, base case on matrix-chain/knapsack/LCS-style tasks.
8. **Greedy vs DP comparison**
   - Especially coin-change counterexample and why greedy fails.
9. **Flow basics**
   - Residual graph, augmenting path, bottleneck, max-flow intuition.
10. **Stable Matching (Gale-Shapley)**
   - One complete hand-trace + stability argument.
11. **QuickSelect / partition**
   - Expected runtime intuition and one manual partition walk.
12. **Final 30-min mixed drill**
   - 2 tracing tasks + 2 asymptotic/runtime tasks + 1 proof-style argument.
