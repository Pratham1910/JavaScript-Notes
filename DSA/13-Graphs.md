# Chapter 13 — Graphs

## 13.1 What & Why

A **Graph** is a set of **vertices (nodes)** connected by **edges**, generalizing trees (a tree is just a graph with no cycles and one path between any two nodes). Graphs can be **directed/undirected**, **weighted/unweighted**, and may contain cycles.

**The problem that created it**: Trees model strict hierarchies, but the real world is full of **many-to-many relationships**: road networks (cities connected by roads, possibly multiple routes, possibly one-way), social networks (friendships, follows), the internet (routers, links), dependency graphs (build systems, course prerequisites), and recommendation systems. None of these fit a tree's "single parent" constraint — graphs were formalized (Euler's Königsberg bridge problem, 1736) specifically to answer connectivity/reachability/path questions on arbitrary relationship networks.

**Real-life example**: Google Maps — cities/intersections are nodes, roads are weighted edges (weight = time/distance). "Shortest route from A to B" is a shortest-path graph algorithm (Dijkstra). "Is there any way to get from A to B" is a reachability/traversal question (BFS/DFS). LinkedIn's "2nd-degree connections" is a BFS from you, 2 levels deep.

## 13.2 Representations

| Representation | Space | Edge lookup (u,v)? | Iterate neighbors of u | Best for |
|---|---|---|---|---|
| Adjacency Matrix | O(V²) | O(1) | O(V) | Dense graphs, small V, frequent edge-existence checks |
| Adjacency List | O(V+E) | O(degree(u)) | O(degree(u)) | Sparse graphs (most real-world graphs), default choice |

## 13.3 Traversals — the foundation of everything else

- **BFS (Breadth-First Search)**: explore level by level using a **queue**. Finds shortest path in **unweighted** graphs. O(V+E).
- **DFS (Depth-First Search)**: explore as deep as possible before backtracking, using a **stack** (explicit or via recursion). Used for cycle detection, topological sort, connected components, path existence. O(V+E).

**Real-life analogy**: BFS = ripples spreading outward from a stone dropped in water (explores all immediate neighbors before going further) — this is *why* it finds shortest paths in unweighted graphs. DFS = exploring a maze by always taking a new corridor until you hit a dead end, then backtracking — good for "does a path exist" and structural analysis, not shortest path.

## 13.4 Core Algorithms

| Algorithm | Purpose | Time | Space | Key idea |
|---|---|---|---|---|
| BFS | Shortest path (unweighted), level order, connectivity | O(V+E) | O(V) | Queue, visit level by level |
| DFS | Path existence, cycle detection, topological order, components | O(V+E) | O(V) | Stack/recursion |
| Dijkstra's | Shortest path (non-negative weights) | O((V+E) log V) with min-heap | O(V) | Greedy: always expand the closest unvisited node (Ch. 11 Heap) |
| Bellman-Ford | Shortest path (handles negative weights, detects negative cycles) | O(V·E) | O(V) | Relax all edges V-1 times |
| Floyd-Warshall | All-pairs shortest path | O(V³) | O(V²) | DP over intermediate vertices |
| Topological Sort (Kahn's / DFS-based) | Ordering respecting dependencies (DAGs only) | O(V+E) | O(V) | In-degree/DFS finish-time based |
| Union-Find (Disjoint Set) | Connectivity, cycle detection in undirected graphs | ~O(α(n)) per op (near O(1)) | O(V) | Path compression + union by rank |
| Kruskal's MST | Minimum Spanning Tree | O(E log E) | O(V) | Sort edges, greedily add if no cycle (Union-Find) |
| Prim's MST | Minimum Spanning Tree | O(E log V) with heap | O(V) | Greedy: grow tree by cheapest connecting edge (like Dijkstra) |
| Cycle Detection (directed) | — | O(V+E) | O(V) | DFS with recursion-stack tracking (white/gray/black coloring) |
| Cycle Detection (undirected) | — | O(V+E) | O(V) | DFS with parent tracking, or Union-Find |

## 13.5 Why so many shortest-path algorithms?

This is a common confusion point — each solves a **different constraint**:
- **BFS**: unweighted graphs only (every edge = 1).
- **Dijkstra**: weighted, but only **non-negative** weights (greedy choice breaks with negative weights, since a "settled" shortest distance could later be beaten by a negative edge).
- **Bellman-Ford**: handles negative weights (slower, O(V·E), but correct); also **detects negative cycles** (which make "shortest path" undefined — Dijkstra can't do this).
- **Floyd-Warshall**: when you need shortest paths between **every pair** of nodes, not just from one source — O(V³) is fine for smaller dense graphs.

## 13.6 Key Patterns

1. **Grid as an implicit graph** — each cell is a node, adjacent cells (4-dir/8-dir) are edges — BFS/DFS on grids solves island-counting, maze shortest path, flood fill.
2. **Multi-source BFS** — start BFS from multiple sources simultaneously (push all into the queue initially) — used for "distance to nearest X" problems (e.g., rotting oranges).
3. **Topological Sort** — order tasks respecting "must come before" dependencies — course scheduling, build systems.
4. **Union-Find** — efficiently track/merge connected components as edges are added — cycle detection, Kruskal's MST, "number of provinces/islands" via union operations.
5. **0-1 BFS** — deque-based BFS for graphs with only weights {0,1}, O(V+E) instead of Dijkstra's O(E log V).
6. **Bidirectional BFS** — search from both source and target simultaneously to cut search space, useful for large unweighted shortest-path queries.

### Code skeleton — BFS
```java
void bfs(int start, Map<Integer, List<Integer>> adj) {
    Set<Integer> visited = new HashSet<>();
    Queue<Integer> queue = new LinkedList<>();
    queue.add(start); visited.add(start);
    while (!queue.isEmpty()) {
        int node = queue.poll();
        visit(node);
        for (int nei : adj.get(node)) {
            if (!visited.contains(nei)) {
                visited.add(nei);
                queue.add(nei);
            }
        }
    }
}
```

### Code skeleton — DFS (recursive)
```java
void dfs(int node, Map<Integer, List<Integer>> adj, Set<Integer> visited) {
    visited.add(node);
    visit(node);
    for (int nei : adj.get(node)) {
        if (!visited.contains(nei)) dfs(nei, adj, visited);
    }
}
```

### Code skeleton — Union-Find with path compression & union by rank
```java
int[] parent, rank_;
int find(int x) {
    if (parent[x] != x) parent[x] = find(parent[x]); // path compression
    return parent[x];
}
void union(int a, int b) {
    int ra = find(a), rb = find(b);
    if (ra == rb) return;
    if (rank_[ra] < rank_[rb]) { int t = ra; ra = rb; rb = t; }
    parent[rb] = ra;
    if (rank_[ra] == rank_[rb]) rank_[ra]++;
}
```

## 13.7 Problems (Basic → Medium → Hard)

### Basic
1. Implement Graph (adjacency list) + BFS + DFS traversal.
2. Number of Connected Components in an Undirected Graph (LeetCode 323).
3. Find if Path Exists in Graph (LeetCode 1971).
4. Flood Fill (LeetCode 733).

### Medium
5. **Number of Islands** (LeetCode 200) — grid BFS/DFS, O(rows·cols).
6. **Rotting Oranges** (LeetCode 994) — multi-source BFS.
7. **Course Schedule I & II** (LeetCode 207, 210) — topological sort / cycle detection in directed graph.
8. **Clone Graph** (LeetCode 133) — BFS/DFS + hashmap.
9. **Pacific Atlantic Water Flow** (LeetCode 417) — multi-source DFS from both borders.
10. **Graph Valid Tree** (LeetCode 261) — Union-Find or DFS cycle check.
11. **Word Ladder** (LeetCode 127) — BFS on implicit word graph.
12. **Network Delay Time** (LeetCode 743) — Dijkstra's algorithm.
13. **Number of Provinces** (LeetCode 547) — Union-Find.
14. **Redundant Connection** (LeetCode 684) — Union-Find cycle detection.
15. **Surrounded Regions** (LeetCode 130) — boundary DFS.
16. **01 Matrix** (LeetCode 542) — multi-source BFS.

### Hard
17. **Alien Dictionary** (LeetCode 269) — build graph from char ordering + topological sort.
18. **Minimum Spanning Tree** (Kruskal's/Prim's) — classic construction, various LeetCode/GfG problems.
19. **Cheapest Flights Within K Stops** (LeetCode 787) — modified Bellman-Ford / BFS with state (node, stops).
20. **Swim in Rising Water** (LeetCode 778) — binary search + BFS, or Dijkstra-variant with min-heap on "max edge on path".
21. **Bus Routes** (LeetCode 815) — BFS on a transformed graph.
22. **Reconstruct Itinerary** (LeetCode 332) — Eulerian path via Hierholzer's algorithm.
23. **Critical Connections in a Network** (LeetCode 1192) — Tarjan's bridge-finding algorithm, O(V+E).
24. **Strongly Connected Components** — Tarjan's or Kosaraju's algorithm, O(V+E).
25. **Floyd-Warshall applications**: Find the City With the Smallest Number of Neighbors at a Threshold Distance (LeetCode 1334).

---
**Prev**: [Tries](12-Trie.md) | **Next**: [Chapter 14 — Dynamic Programming](14-DP.md)
