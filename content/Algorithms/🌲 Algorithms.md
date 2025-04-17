Algorithms let us think about computation independently of the limitations of a computer or programming language.

1. [[🔍 Binary Search]] efficiently finds a target in a sorted list by halving the search range each step.
2. [[👉👈 Two Pointers]] uses two indices moving toward each other (or in tandem) to solve array problems in linear time.
3. [[🔀 Sorting]] reorganizes elements into a specified order (e.g. ascending) using algorithms like quicksort or mergesort.
4. [[🪟 Sliding Window]] maintains a window over a sequence to compute metrics (sum, max, etc.) for all subarrays of fixed size in $O(n)$.
5. [[➕ Prefix Sums]] precomputes cumulative sums, allowing us to query any subarray total in constant time.
6. [[♾️ Recursion]] solves problems by having functions call themselves on progressively smaller inputs, often simplifying divide‑and‑conquer.
7. [[💧 Breadth First Search]] explores a graph level by level, ideal for finding shortest paths in unweighted graphs.
8. [[🚟 Depth First Search]] dives deep along one branch before backtracking, useful for connectivity and topological sort.
9. [[🔄 Dynamic Programming]] breaks problems into overlapping subproblems, caching results to avoid redundant work.
10. [[🗺️ Dijkstra's Algorithm]] finds shortest paths from a source in a non-negative weighted graph using a priority queue.
11. [[🚢 Prim's and Kruskal's Algorithm]] greedily builds a minimum spanning tree by selecting the lightest edges without creating cycles.
12. [[🥽 Union-Find]] manages disjoint sets with near‑constant time union and find operations (useful for connectivity).
13. [[🃏 Randomized Search Tree]] employs randomness to simplify solutions or improve expected performance on average.
# Data Structures
1. [[📍 Hashmap]]s store key–value pairs for average $O(1)$ lookup, insertion, and deletion.
2. [[🍽️ Stacks]] provide a LIFO structure supporting push and pop at one end, handy for backtracking and parsing.
3. [[🏁 Queues]] provide a FIFO structure supporting enqueue at rear and dequeue from front, ideal for BFS.
4. [[⛓️ Linked List]]s provide a sequence of nodes where each node points to the next, allowing $O(1)$ insertion/deletion with a pointer.
5. [[📊 Graphs]] model entities (nodes) and their relationships (edges), foundational for network and connectivity problems.
6. [[🌳 Binary Search Tree]]s are binary trees where left child < node < right child, giving $O(\log n)$ search on average.
7. [[⛰️ Heaps]] are a tree‑based priority queue that lets us extract the max (or min) in $O(\log n)$.
8. [[🔴 Red-Black Tree]] is a self‑balancing BST ensuring $O(\log n$) operations by enforcing color and rotation invariants.
9. [[☮️ AVL Tree]] is a height‑balanced BST with strict balance factor, guaranteeing $O(\log n)$ in worst case.
10. [[🎲 Treap]] is a randomized BST combining heap priorities with BST keys to maintain balance probabilistically.
11. [[📐 K-Dimensional Tree]]s partition k‑dimensional points recursively, useful for nearest neighbor searches.
12. [[📂 B-Trees and B+ Trees]] provide wide, multi‑way trees optimized for disk and block storage, minimizing I/O operations.