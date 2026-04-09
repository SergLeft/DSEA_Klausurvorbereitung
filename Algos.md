# Comprehensive List of Algorithms and Data Structures for DSEA

## 1. Data Structures

### Binary Heaps (Min-Heap / Max-Heap)
**Best Suited:** Priority queues, repeatedly finding/extracting the minimum or maximum element efficiently (e.g., in Dijkstra's or Prim's algorithms).
**Step-by-Step Explanation:**
1. Stored as an array where children of node `i` are at `2i` and `2i+1`.
2. **Insert:** Place at the end of the array, then `trickle-up` (swap with parent if smaller).
3. **Extract-Min:** Swap root with the last element, remove the last, then `trickle-down` (swap with the smallest child) to restore the heap property.
**Pseudocode:**
```python
class MinHeap:
    def insert(self, val):
        self.heap.append(val)
        self._trickle_up(len(self.heap) - 1)

    def extract_min(self):
        self.heap[0], self.heap[-1] = self.heap[-1], self.heap[0]
        min_val = self.heap.pop()
        self._trickle_down(0)
        return min_val
```
**Time Complexity & Proof:** 
- *Insert/Extract:* O(log n). The height of a complete binary tree is log n. Elements traverse at most the height of the tree.
- *Build-Heap:* O(n). Proven via sum of heights: most nodes are at the bottom and barely move, bounded by a converging geometric series.

### AVL Trees (Self-Balancing BST)
**Best Suited:** Fast lookups, insertions, and deletions where dynamic ordered data is required without degrading to a linked list (O(n)).
**Step-by-Step Explanation:**
1. Standard BST insertion.
2. Update heights and calculate the balance factor (Height(Left) - Height(Right)).
3. If unbalanced (factor > 1 or < -1), perform 1 or 2 rotations (LL, RR, LR, RL) to restore balance.
**Pseudocode:**
```python
def insert(node, key):
    if not node: return Node(key)
    if key < node.key: node.left = insert(node.left, key)
    else: node.right = insert(node.right, key)
    
    balance = get_balance(node)
    # Perform rotations based on balance factor (LL, RR, LR, RL cases)
    return rebalance(node, balance, key)
```
**Time Complexity & Proof:** 
O(log n) for all operations. The balance condition ensures the height `h` is bounded by O(log n) strictly (proven via Fibonacci sequence relationship to minimum nodes for height h).

### Union-Find (Disjoint Set Union - DSU)
**Best Suited:** Grouping items into disjoint sets and quickly checking if two items are in the same set (e.g., cycle detection in Kruskal's).
**Step-by-Step Explanation:**
1. **Find:** Follow parent pointers to the root. Use *Path Compression* (make nodes point directly to the root).
2. **Union:** Attach the root of the smaller tree to the root of the larger tree (*Union by Rank*).
**Pseudocode:**
```python
def find(i):
    if parent[i] == i: return i
    parent[i] = find(parent[i]) # Path compression
    return parent[i]

def union(i, j):
    root_i, root_j = find(i), find(j)
    if root_i != root_j:
        parent[root_i] = root_j # Simplified, omitting rank for brevity
```
**Time Complexity & Proof:** 
O(α(n)) amortized, where α is the inverse Ackermann function (effectively O(1)). Proven via amortized analysis of path compression.

---

## 2. Graph Algorithms

### Dijkstra's Algorithm
**Best Suited:** Single-source shortest paths in graphs with **non-negative** edge weights.
**Step-by-Step:**
1. Set distance to start = 0, all others to infinity. Insert into a Priority Queue (Min-Heap).
2. Extract the minimum distance node.
3. Relax all its outgoing edges: if `dist[u] + weight(u, v) < dist[v]`, update `dist[v]` and the Queue.
**Pseudocode:**
```python
def dijkstra(graph, start):
    pq = MinHeap([(0, start)])
    dist = {start: 0}
    while pq:
        d, u = pq.extract_min()
        for v, weight in graph[u]:
            if d + weight < dist.get(v, inf):
                dist[v] = d + weight
                pq.insert((dist[v], v))
```
**Time Complexity & Proof:** O((V + E) log V) using a Binary Heap. Each vertex is extracted once (V log V), and each edge is relaxed once (E log V).

### Bellman-Ford Algorithm
**Best Suited:** Single-source shortest paths when **negative weights** exist. Can detect negative cycles.
**Step-by-Step:**
1. Relax all edges E in the graph.
2. Repeat step 1 exactly V-1 times.
3. Check for negative cycles by iterating all edges one last time (if a distance drops, there is a cycle).
**Time Complexity & Proof:** O(V * E). Two nested loops iterating V-1 times over E edges.

### Floyd-Warshall Algorithm
**Best Suited:** All-pairs shortest paths.
**Step-by-Step:**
1. DP formulation: Can we get a shorter path from i to j by routing through k?
2. 3 nested loops (k, i, j). `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`.
**Time Complexity & Proof:** O(V^3). Three nested loops iterating 1 to V.

### Kruskal's vs Prim's vs Borůvka's (Minimum Spanning Trees)
- **Kruskal's:** Sort edges by weight. Iterate and add to MST if it doesn't form a cycle (using Union-Find). 
  *Time Complexity:* O(E log E) due to sorting.
- **Prim's:** Start at an arbitrary node. Grow the tree by always taking the cheapest edge connecting the tree to a new node (uses Min-Heap).
  *Time Complexity:* O((V + E) log V).
- **Borůvka's:** Every node starts as its own component. In each round, every component simultaneously picks its cheapest outgoing edge. Merge components. Repeat.
  *Time Complexity:* O(E log V). Halves the number of components each round.

### Topological Sort
**Best Suited:** Ordering a Directed Acyclic Graph (DAG) such that for every directed edge u->v, u comes before v.
**Step-by-Step:**
1. Compute in-degrees for all nodes.
2. Put nodes with 0 in-degree in a queue.
3. Pop, append to sorted list, and decrement in-degrees of neighbors. If a neighbor reaches 0, push to queue.
**Time Complexity:** O(V + E) since every vertex and edge is visited exactly once.

---

## 3. Divide and Conquer

### MergeSort
**Best Suited:** General purpose stable sorting, especially linked lists or external sorting.
**Time Complexity & Proof:** O(n log n) Worst-case. Recurrence T(n) = 2T(n/2) + O(n). Master Theorem Case 2.

### QuickSort & QuickSelect
**Best Suited:** In-place sorting (QuickSort). Finding the k-th smallest element in linear time (QuickSelect).
**Step-by-Step (QuickSelect):**
1. Pick a pivot. Partition array into `< pivot`, `== pivot`, `> pivot`.
2. Count elements in the left partition. If k <= left size, recurse left. Else if k > left + equal, recurse right (adjusting k). Else, return pivot.
**Time Complexity & Proof:** 
- QuickSort: Expected O(n log n), Worst O(n^2). Proven via harmonic sum of comparison probabilities.
- QuickSelect: Expected O(n). Recurrence roughly T(n) = T(n/2) + O(n) which sums to O(n).

### Fast Fourier Transform (FFT)
**Best Suited:** Multiplying polynomials in sub-quadratic time.
**Step-by-Step:**
1. Evaluate polynomials at complex roots of unity (O(n log n) via divide and conquer separating even and odd coefficients).
2. Pointwise multiply the results (O(n)).
3. Inverse FFT to get coefficients back (O(n log n)).
**Time Complexity & Proof:** O(n log n). Recurrence T(n) = 2T(n/2) + O(n).

### Karatsuba Multiplication
**Best Suited:** Multiplying very large integers.
**Explanation:** Splits numbers into halves to compute 3 multiplications instead of 4.
**Time Complexity:** O(n^(log_2 3)) ≈ O(n^1.58).

---

## 4. Dynamic Programming (DP)

### Matrix Chain Multiplication
**Best Suited:** Finding the optimal parenthesis placement to multiply a chain of matrices.
**Step-by-Step:** 
State `dp[i][j]` = min cost to multiply matrices from `i` to `j`. Iterate over chain lengths from 2 to n.
**Time Complexity:** O(n^3) due to 3 loops (length, start pos, split point).

### Longest Palindromic Subsequence (LPS)
**Best Suited:** Finding the longest subsequence that is a palindrome.
**Pseudocode:**
```python
def lps(s):
    # dp[i][j] = LPS of substring s[i..j]
    if s[i] == s[j]: dp[i][j] = 2 + dp[i+1][j-1]
    else: dp[i][j] = max(dp[i+1][j], dp[i][j-1])
```
**Time Complexity:** O(n^2) space and time.

### 0/1 Knapsack
**Best Suited:** Maximizing value within a weight limit where items cannot be broken.
**Time Complexity:** O(nW) pseudo-polynomial.

---

## 5. Greedy Algorithms

### Huffman Coding
**Best Suited:** Creating optimal prefix-free codes for data compression.
**Step-by-Step:**
1. Put all character frequencies in a Min-Heap.
2. Extract the two smallest, merge them into a parent node with sum frequency, and push back.
3. Repeat until 1 tree remains.
**Time Complexity:** O(n log n) due to priority queue operations.

### Fractional Knapsack
**Best Suited:** Knapsack where items *can* be broken.
**Explanation:** Sort by value-to-weight ratio and take greedily.
**Time Complexity:** O(n log n) due to sorting.

---

## 6. Flow Networks & Stable Matching

### Ford-Fulkerson Algorithm
**Best Suited:** Finding the maximum flow in a network.
**Step-by-Step:**
1. Find an augmenting path from Source to Sink in the Residual Network.
2. Find the bottleneck capacity of that path.
3. Augment flow: add to forward edges, subtract from backward edges. Repeat until no paths exist.
**Time Complexity:** O(max_flow * E).

### Gale-Shapley (Deferred Acceptance)
**Best Suited:** Stable matching (e.g., university admissions).
**Explanation:** Proposers propose to their top choice. Receivers tentatively accept the best offer and reject others. Rejects move to their next choice.
**Time Complexity:** O(n^2) worst case as no proposer proposes to the same receiver twice.
