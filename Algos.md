# Algorithms and Data Structures (Cross-Checked from Notes)

This file consolidates algorithms and data structures found in:
- `DSEA_notes.md`
- `SolutionsAltklausuren.md`
- `SolutionsHA.md`
- `SolutionsPrev.md`
- `StudyGuide.md`

For each item: best use case, short steps, Python-style pseudocode, and complexity with derivation idea.

---

## A) Data Structures

### 1) Binary Heap (Min-Heap / Max-Heap)
**Best suited:** Fast repeated min/max extraction (priority queues, Dijkstra/Prim/Huffman).
**Steps:** Store as complete binary tree in array; insert at end + sift-up; extract root + swap with last + sift-down.
**Pseudocode:**
```python
def push(h, x):
    h.append(x); sift_up(h, len(h)-1)

def pop_min(h):
    h[0], h[-1] = h[-1], h[0]
    x = h.pop(); sift_down(h, 0); return x
```
**Time:** `push/pop = O(log n)` (move along tree height), `build_heap = O(n)` (sum of node heights).

### 2) Priority Queue
**Best suited:** Always process next most urgent/min-cost element.
**Steps:** Backed by heap/buckets; push, decrease-key (if supported), pop-min/pop-max.
**Pseudocode:**
```python
pq = []
push(pq, (prio, item))
prio, item = pop_min(pq)
```
**Time:** Heap implementation: `O(log n)` updates/extracts.

### 3) Bucket Queue
**Best suited:** Integer priorities in bounded range `[0..k]` (e.g., bounded-weight MST/SP variants).
**Steps:** Array of buckets by key; maintain current non-empty bucket pointer.
**Pseudocode:**
```python
buckets = [list() for _ in range(k+1)]
def push(x, key): buckets[key].append(x)
def pop_min():
    while not buckets[cur]: cur += 1
    return buckets[cur].pop()
```
**Time:** Often `O(1)` amortized per op, total near `O(k + m)` scan + inserts/extracts.

### 4) Dynamic Array
**Best suited:** Random-access array with amortized `O(1)` append.
**Steps:** On full capacity, allocate larger array and copy; optionally shrink when sparse.
**Pseudocode:**
```python
def append(x):
    if n == cap: resize(2*cap)
    a[n] = x; n += 1
```
**Time:** Append amortized `O(1)` via geometric resizing; occasional resize costs spread over many appends.

### 5) Stack
**Best suited:** LIFO processing (DFS, parsing, undo, monotonic tricks).
**Steps:** push/pop at one end.
**Pseudocode:**
```python
st.append(x)
x = st.pop()
```
**Time:** `O(1)` per operation.

### 6) Queue
**Best suited:** FIFO processing (BFS, scheduling).
**Steps:** enqueue at tail, dequeue at head.
**Pseudocode:**
```python
q.append(x)
x = q.popleft()
```
**Time:** `O(1)` with deque/ring buffer.

### 7) Hash Table / Hash Map
**Best suited:** Expected `O(1)` insert/find/delete.
**Steps:** Hash key -> bucket/slot; resolve collisions.
**Pseudocode:**
```python
def put(k, v):
    i = h(k) % m
    place_or_resolve(i, k, v)
```
**Time:** Expected `O(1)` under uniform hashing and bounded load factor.

### 8) Open Addressing (Linear Probing / Double Hashing)
**Best suited:** Hashing without pointer chains.
**Steps:** On collision probe sequence until empty/match.
**Pseudocode:**
```python
def find_slot(k):
    i = h1(k) % m
    step = 1              # linear; for double hashing use h2(k)
    while table[i] not in (EMPTY, k): i = (i + step) % m
    return i
```
**Time:** Expected `O(1)` at low/moderate load; worsens near full table.

### 9) Cuckoo Hashing
**Best suited:** Worst-case `O(1)` lookup with two hash locations.
**Steps:** Insert by evicting occupant to its alternate position; rebuild on long cycles.
**Pseudocode:**
```python
def insert(x):
    for _ in range(limit):
        pos = hA(x) if inA else hB(x)
        x, table[pos] = table[pos], x
        if x is EMPTY: return
    rehash()
```
**Time:** Lookup `O(1)` worst-case; insert expected `O(1)` (rehash rare amortized).

### 10) Universal / 2-Universal Hash Family
**Best suited:** Provable low collision probability in randomized algorithms.
**Steps:** Randomly choose hash function from family; use in hash table/minhash etc.
**Pseudocode:**
```python
choose a,b uniformly
h(x) = ((a*x + b) % p) % m
```
**Time:** Hash eval `O(1)`; guarantee: `Pr[h(x)=h(y)] <= 1/m` (2-universal).

### 11) Binary Search Tree (BST)
**Best suited:** Ordered set/map with predecessor/successor queries.
**Steps:** Left < node < right recursively.
**Pseudocode:**
```python
def search(t, x):
    if not t or t.key == x: return t
    return search(t.left, x) if x < t.key else search(t.right, x)
```
**Time:** `O(h)`, worst `O(n)` if unbalanced.

### 12) AVL Tree
**Best suited:** Guaranteed logarithmic ordered operations.
**Steps:** BST insert/delete + update heights + rotations (LL/RR/LR/RL).
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
**Steps:** `find` root with path compression; `union` by rank/size.
**Pseudocode:**
```python
def find(x):
    if p[x] != x: p[x] = find(p[x])
    return p[x]

def union(a,b):
    ra, rb = find(a), find(b)
    if ra != rb: link_by_rank(ra, rb)
```
**Time:** Amortized `O(alpha(n))` per op (inverse Ackermann).

### 14) Residual Network (flow DS)
**Best suited:** Represent current flow + possible augment/reverse moves.
**Steps:** Maintain residual capacities for forward/backward edges.
**Pseudocode:**
```python
res[u][v] = cap[u][v] - flow[u][v]
res[v][u] = flow[u][v]
```
**Time:** Structure update `O(path length)` after augmentation.

### 15) Prefix-Free Code Tree (Huffman/Shannon-Fano output structure)
**Best suited:** Decodable variable-length coding.
**Steps:** Build binary tree; leaf path bits are codewords.
**Pseudocode:**
```python
def decode(bits, root):
    u = root
    for b in bits:
        u = u.left if b == 0 else u.right
```
**Time:** Decode `O(number_of_bits)`.

---

## B) Core Graph / Flow Algorithms

### 16) BFS (Breadth-First Search)
**Best suited:** Unweighted shortest paths, layering, bipartite test.
**Steps:** Queue from source; visit neighbors level by level.
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
**Steps:** Recurse/stack deeply then backtrack.
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
**Steps:** Run BFS/DFS from each unvisited node, assign component id.
**Pseudocode:**
```python
cid = 0
for u in V:
    if u not in vis: bfs_label(u, cid); cid += 1
```
**Time:** `O(V+E)`.

### 19) Dijkstra
**Best suited:** Single-source shortest paths with nonnegative edges.
**Steps:** Dist init; repeatedly settle min-distance node; relax outgoing edges.
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
**Steps:** Relax all edges `V-1` rounds; one extra round checks cycle.
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
**Steps:** DP over allowed intermediate vertices `k`.
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
**Steps:** Sort edges by weight; add if endpoints in different DSU sets.
**Pseudocode:**
```python
E.sort(key=lambda e: e.w)
for e in E:
    if find(e.u) != find(e.v): union(e.u, e.v); mst.append(e)
```
**Time:** `O(E log E)` sorting dominates; DSU almost constant amortized.

### 23) Prim MST
**Best suited:** MST by growing one tree from any start node.
**Steps:** Keep min crossing edge via priority queue.
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
**Steps:** Each component picks cheapest outgoing edge; merge; repeat.
**Pseudocode:**
```python
while num_components > 1:
    cheapest = pick_cheapest_outgoing_per_component()
    for e in cheapest: if find(e.u) != find(e.v): union(e.u, e.v)
```
**Time:** `O(E log V)`; components at least halve each round.

### 25) Topological Sort (Kahn)
**Best suited:** Linearization of DAG dependencies.
**Steps:** Compute indegrees; queue zero-indegree nodes; remove iteratively.
**Pseudocode:**
```python
q = deque([u for u in V if indeg[u] == 0])
while q:
    u = q.popleft(); order.append(u)
    for v in G[u]: indeg[v] -= 1; 
    if indeg[v] == 0: q.append(v)
```
**Time:** `O(V+E)`.

### 26) Ford-Fulkerson (augmenting paths)
**Best suited:** Max flow foundation.
**Steps:** Repeatedly find augmenting path in residual graph, augment by bottleneck.
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
**Steps:** Ford-Fulkerson with shortest augmenting path in edge count.
**Pseudocode:**
```python
while path := bfs_residual_path(s, t):
    b = bottleneck(path); augment(path, b)
```
**Time:** `O(VE^2)` (each BFS `O(E)`, critical-edge argument bounds augmentations).

### 28) Cycle-Canceling in Flows
**Best suited:** Convert arbitrary feasible flow to acyclic flow / reduce circulation cost cases.
**Steps:** Detect positive-flow directed cycle; push minimum cycle flow backward; repeat.
**Pseudocode:**
```python
while cycle := find_positive_flow_cycle():
    d = min(flow[e] for e in cycle)
    for e in cycle: flow[e] -= d
```
**Time:** Polynomial with proper cycle detection; each cancel removes at least one positive edge on that cycle.

### 29) Bipartite Matching via Max Flow
**Best suited:** Maximum cardinality matching in bipartite graphs.
**Steps:** Source -> left (cap1), left->right edges (cap1), right->sink (cap1), run max flow.
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
**Steps:** Divide in halves, recursively sort, merge two sorted halves.
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
**Steps:** Pick pivot, partition, recurse on sides.
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
**Steps:** Partition by pivot; recurse only side containing k.
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
**Steps:** Repeatedly select minimum of unsorted suffix and swap front.
**Pseudocode:**
```python
for i in range(n):
    j = argmin(a[i:])
    a[i], a[j] = a[j], a[i]
```
**Time:** `O(n^2)` comparisons (`n+(n-1)+...+1`).

### 34) Insertion Sort
**Best suited:** Nearly sorted arrays, small base cases.
**Steps:** Insert each element into sorted prefix.
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
**Steps:** Count frequencies, prefix sums, place stably.
**Pseudocode:**
```python
cnt = [0]*(k+1)
for x in a: cnt[x] += 1
for i in range(1, k+1): cnt[i] += cnt[i-1]
```
**Time:** `O(n+k)` (linear in elements + key range).

### 36) Radix Sort (incl. MSD Radix Sort)
**Best suited:** Fixed-length strings/integers with bounded alphabet/base.
**Steps:** Sort by digits/chars (LSD or recursive MSD buckets).
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
**Steps:** Recursively sort overlapping/partial segments of size about `2n/3`.
**Pseudocode:**
```python
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
**Steps:** Compare with midpoint; discard half each iteration.
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
**Steps:** Scan left to right until match/end.
**Pseudocode:**
```python
for i, v in enumerate(a):
    if v == x: return i
return -1
```
**Time:** `O(n)` worst; expected `O(n)` without extra distribution assumptions.

### 40) Two-Pointer Technique
**Best suited:** Sorted-array pair/triplet constraints, window-like scans.
**Steps:** Maintain two indices and move based on condition.
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
**Steps:** Expand right, shrink left while constraint violated.
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
**Steps:** Cancel out different elements via counter.
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
**Steps:** Pairwise eliminate impossible candidates, then verify final candidate.
**Pseudocode:**
```python
c = 0
for i in range(1, n):
    if knows(c, i): c = i
verify(c)
```
**Time:** `O(n)` queries for elimination + `O(n)` verification.

---

## D) Divide-and-Conquer / Numeric

### 44) DFT (Discrete Fourier Transform)
**Best suited:** Transform polynomial coefficients <-> value form.
**Steps:** Evaluate polynomial at roots of unity.
**Pseudocode:**
```python
def dft(a, w):
    return [sum(a[j] * (w**(k*j)) for j in range(n)) for k in range(n)]
```
**Time:** Direct DFT `O(n^2)` (double summation).

### 45) FFT (Fast Fourier Transform, Cooley-Tukey)
**Best suited:** Fast polynomial multiplication and convolution.
**Steps:** Split even/odd coefficients recursively; combine with twiddle factors.
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
**Steps:** Split numbers high/low; do 3 recursive multiplies instead of 4.
**Pseudocode:**
```python
def kara(x, y):
    if small(x,y): return x*y
    a,b,c,d = split(x,y)
    ac = kara(a,c); bd = kara(b,d); s = kara(a+b, c+d)
    return ac*B2 + (s-ac-bd)*B + bd
```
**Time:** `T(n)=3T(n/2)+O(n)` => `O(n^{log2 3})`.

### 47) Strassen Matrix Multiplication
**Best suited:** Large dense matrix multiplication.
**Steps:** Block matrices; compute 7 subproducts (not 8) + recombination.
**Pseudocode:**
```python
def strassen(A, B):
    if small: return naive_mul(A,B)
    split blocks; compute M1..M7; combine to C
```
**Time:** `T(n)=7T(n/2)+O(n^2)` => `O(n^{log2 7})`.

---

## E) Dynamic Programming and Greedy

### 48) Matrix Chain Multiplication (DP)
**Best suited:** Optimal parenthesization of matrix product chain.
**Steps:** `dp[i][j] = min_k dp[i][k] + dp[k+1][j] + cost`.
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
**Steps:** `dp[i][j]` from prefixes; match => `+1`, else max of neighbors.
**Pseudocode:**
```python
if a[i-1] == b[j-1]: dp[i][j] = dp[i-1][j-1] + 1
else: dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```
**Time:** `O(nm)` table fill.

### 50) Hirschberg (space-optimized LCS reconstruction)
**Best suited:** Recover LCS string with linear extra memory.
**Steps:** Divide one string, run forward/backward DP rows, split optimally, recurse.
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
**Steps:** Interval DP on `i..j`; equal ends => `2+dp[i+1][j-1]` else max neighbors.
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
**Steps:** Include/exclude transition on item index and remaining capacity.
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
**Steps:** Sort by value/weight ratio descending; take full then fraction.
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
**Steps:** `dp[len] = max(price[i] + dp[len-i])`.
**Pseudocode:**
```python
for L in range(1, n+1):
    dp[L] = max(price[i] + dp[L-i] for i in range(1, L+1))
```
**Time:** `O(n^2)`.

### 55) Coin Change
**Best suited:** Minimum coins / counting ways; also compare greedy vs DP correctness.
**Steps:** DP over amount; transition by coin usage.
**Pseudocode:**
```python
dp = [INF]*(A+1); dp[0] = 0
for x in range(1, A+1):
    dp[x] = min((dp[x-c]+1 for c in coins if c<=x), default=INF)
```
**Time:** `O(A * #coins)`; greedy may fail on non-canonical systems.

### 56) Interval Scheduling (Greedy)
**Best suited:** Max number of non-overlapping intervals.
**Steps:** Sort by finish time; repeatedly choose first compatible.
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
**Steps:** Define `dp[pos]` = optimal value up to position; transition by intervals ending at `pos`.
**Pseudocode:**
```python
for p in range(1, M+1):
    dp[p] = best_transition_from_intervals_covering(p)
```
**Time:** Depends on transition implementation (often `O(M + total_intervals)` or `O(M log M)`).

### 58) Bookshelf DP (exam variant)
**Best suited:** Partition/order optimization with shelf constraints.
**Steps:** `dp[i]` best for first `i` books; try last shelf start `j`.
**Pseudocode:**
```python
for i in range(1, n+1):
    dp[i] = min(cost(j,i) + dp[j-1] for j in feasible_starts(i))
```
**Time:** Typically `O(n^2)` unless optimized.

---

## F) Coding, Matching, Randomized / Specialized

### 59) Huffman Coding
**Best suited:** Optimal prefix-free coding for known symbol frequencies.
**Steps:** Repeatedly merge two lowest-frequency trees in min-heap.
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
**Steps:** Sort by frequency; recursively split into two near-equal frequency groups.
**Pseudocode:**
```python
def sf(symbols):
    if len(symbols) <= 1: return
    L, R = split_near_half_weight(symbols)
    prefix0(L); prefix1(R); sf(L); sf(R)
```
**Time:** Sorting dominates `O(k log k)`.

### 61) Gale-Shapley / Deferred Acceptance (Stable Matching)
**Best suited:** Stable matching (one-to-one or capacity variants).
**Steps:** Free proposers propose next choice; acceptor keeps best current proposal.
**Pseudocode:**
```python
while free_proposer_exists():
    a = pick_free(); b = next_choice[a]
    if b free or b prefers a over current: engage(a,b); free(old_partner)
```
**Time:** `O(n^2)` proposals upper bound (each pair proposed at most once).

### 62) MinHash
**Best suited:** Fast approximate Jaccard similarity at scale.
**Steps:** Apply random permutations/hash functions; keep minimum hash per set; compare signatures.
**Pseudocode:**
```python
def minhash_sig(S, hs):
    return [min(h(x) for x in S) for h in hs]
```
**Time:** `O(k*|S|)` for `k` hash functions; estimator variance decreases as `1/k`.

### 63) Jaccard Similarity Computation
**Best suited:** Exact set similarity baseline.
**Steps:** Compute intersection and union sizes.
**Pseudocode:**
```python
J = len(A & B) / len(A | B)
```
**Time:** Hash-set implementation `O(|A|+|B|)` expected.

### 64) Von Neumann Extractor
**Best suited:** Build fair coin from unknown biased coin (independent tosses).
**Steps:** Toss twice; HT->1, TH->0, HH/TT retry.
**Pseudocode:**
```python
def fair_bit():
    while True:
        a, b = biased(), biased()
        if (a,b)==('H','T'): return 1
        if (a,b)==('T','H'): return 0
```
**Time:** Constant expected tosses (`1/(p(1-p))` pair-trials in expectation).

### 65) Rejection Sampling (discrete)
**Best suited:** Simulate target distribution from easy base randomness.
**Steps:** Sample candidate uniformly from larger range; reject out-of-range events.
**Pseudocode:**
```python
def sample_1_over_n(n):
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
