# Comprehensive List of Algorithms and Data Structures

## Heaps
### Best Suited
For priority queues, finding min/max.

### Step-by-Step Explanation
1. Insert an element: Add to the end and bubble up.
2. Remove min/max: Swap with last and bubble down.

### Pseudocode
```python
class MinHeap:
    def insert(self, value):
        # Insert value into the heap

    def remove(self):
        # Remove and return min value
```
### Time Complexity
- Insert: O(log n)
- Remove: O(log n)

---

## AVL Trees
### Best Suited
When balanced binary search trees are required.

### Step-by-Step Explanation
1. Insert a node.
2. Rotate to balance if necessary.

### Pseudocode
```python
class AVLTree:
    def insert(self, value):
        # Insert value and rebalance
```
### Time Complexity
O(log n) for Search, Insert, Delete.

---

## Hash Tables
### Best Suited
For quick look-up and storage.

### Step-by-Step Explanation
1. Compute hash index.
2. Handle collisions.

### Pseudocode
```python
class HashTable:
    def insert(self, key, value):
        # Insert key-value pair
```
### Time Complexity
- Average: O(1)
- Worst: O(n)

---

## Union-Find
### Best Suited
To find connected components.

### Step-by-Step Explanation
1. Path compression for find.
2. Union by rank.

### Pseudocode
```python
class UnionFind:
    def find(self, item):
        # Find root of item
```
### Time Complexity
O(α(n)), where α is the Inverse Ackermann function.

---

## Dijkstra's Algorithm
### Best Suited
For shortest paths in weighted graphs.

### Step-by-Step Explanation
1. Initialize distances.
2. Update neighboring nodes.

### Pseudocode
```python
def dijkstra(graph, start):
    # Return shortest paths from start
```
### Time Complexity
- O(V^2) or O(E + V log V) with priority queue.

---

## Floyd-Warshall Algorithm
### Best Suited
For finding shortest paths in dense graphs.

### Step-by-Step Explanation
1. Initialize distance matrix.
2. Relax edges iteratively.

### Pseudocode
```python
def floyd_warshall(graph):
    # Return shortest path matrix
```
### Time Complexity
O(V^3)

---

## Kruskal's Algorithm
### Best Suited
For minimum spanning tree.

### Step-by-Step Explanation
1. Sort edges.
2. Use Union-Find to check cycles.

### Pseudocode
```python
def kruskal(graph):
    # Return minimum spanning tree
```
### Time Complexity
O(E log E) or O(E log V).

---

## Prim's Algorithm
### Best Suited
For minimum spanning tree.

### Step-by-Step Explanation
1. Start with a vertex.
2. Expand tree by minimum edge.

### Pseudocode
```python
def prim(graph, start):
    # Return minimum spanning tree
```
### Time Complexity
O(E + log V) with priority queue.

---

## BFS/DFS
### Best Suited
For traversing trees/graphs.

### Step-by-Step Explanation
1. BFS uses a queue; DFS uses a stack.

### Pseudocode
```python
def bfs(graph, start):
    # Breadth-first search
```
### Time Complexity
O(V + E)

---

## Ford-Fulkerson Algorithm
### Best Suited
For maximum flow problems.

### Step-by-Step Explanation
1. Find augmenting paths.
2. Update flows.

### Pseudocode
```python
def ford_fulkerson(graph, source, sink):
    # Return maximum flow
```
### Time Complexity
O(max_flow * E)

---

## Gale-Shapley Algorithm
### Best Suited
For stable marriages.

### Step-by-Step Explanation
1. Propose and reject method.

### Pseudocode
```python
def gale_shapley(men, women):
    # Return stable matches
```
### Time Complexity
O(n^2)

---

## MergeSort
### Best Suited
For stable sorting.

### Step-by-Step Explanation
1. Split list into halves.
2. Merge sorted halves.

### Pseudocode
```python
def merge_sort(arr):
    # Return sorted array
```
### Time Complexity
O(n log n)

---

## QuickSort
### Best Suited
For in-place sorting.

### Step-by-Step Explanation
1. Choose pivot.
2. Partition around pivot.

### Pseudocode
```python
def quick_sort(arr):
    # Return sorted array
```
### Time Complexity
O(n log n) on average.

---

## QuickSelect
### Best Suited
For selecting the k-th smallest element.

### Step-by-Step Explanation
1. Similar to QuickSort.
2. Partitions to find k-th.

### Pseudocode
```python
def quick_select(arr, k):
    # Return k-th smallest element
```
### Time Complexity
O(n) on average.

---

## Fast Fourier Transform (FFT)
### Best Suited
For polynomial multiplication.

### Step-by-Step Explanation
1. Decompose into smaller FFTs.
2. Combine results.

### Pseudocode
```python
def fft(arr):
    # Return FFT of array
```
### Time Complexity
O(n log n)

---

## Knapsack Dynamic Programming
### Best Suited
0/1 knapsack problem.

### Step-by-Step Explanation
1. Build DP table based on weights and values.

### Pseudocode
```python
def knapsack(weights, values, capacity):
    # Return maximum value
```
### Time Complexity
O(nW) where W is the maximum weight.