[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/I7NCKCh8)
# Week 10 Coding #8: Haunted Hotel Sweep

## Summary

This assignment models a haunted hotel as an **undirected graph**, where each room or area is a node and each hallway, staircase, or door is an edge. The graph is stored as an adjacency list (a dictionary where each key is an area and its value is a list of connected areas).

**BFS (Breadth-First Search)** explores the hotel level by level using a queue — it visits all areas one step away before moving further. This is useful for finding the shortest path or sweeping nearby rooms first.

**DFS (Depth-First Search)** explores the hotel by going as deep as possible along one path before backtracking, using a stack. This is useful for exploring entire corridors before moving on.

The `visited` set is essential in both BFS and DFS. Without it, the traversal would loop forever in areas that form cycles (e.g. Lobby → Hallway → Library → Cellar → Kitchen → Dining Room → Lobby). Marking visited areas ensures each area is processed only once.

---

## Approach

- **`get_neighbors`**: Used `dict.get(area, [])` to safely return neighbors. If the area is not in the graph, it returns an empty list instead of raising a `KeyError`.

- **`has_path`**: Used DFS with a stack and a `visited` set. Before starting, checked if both `start` and `target` exist in the graph. If the current node matches the target, returned `True`. If the full traversal completes without finding it, returned `False`.

- **`bfs_order`**: Used a `deque` as the queue. Added `start` to both the queue and `visited` set upfront, then repeatedly popped from the left, appended to the result list, and enqueued unvisited neighbors.

- **`dfs_order`**: Used a list as a stack. Popped from the end, skipped already-visited nodes, then pushed neighbors in **reversed order** so that the first neighbor in the original list is explored first (since a stack processes last-in first-out).

- **Preventing repeated visits**: Both BFS and DFS use a `visited` set. A node is added to `visited` as soon as it is confirmed to be processed, preventing it from being visited again even if it appears as a neighbor of multiple nodes.

---

## Complexity

### `get_neighbors`

- Time: O(1) average
- Space: O(1)
- Why: Dictionary lookup by key is O(1) on average in Python. No extra space is used beyond returning the existing list reference.

### `has_path`

- Time: O(V + E)
- Space: O(V)
- Why: In the worst case, every vertex (V) and every edge (E) is visited once. The `visited` set and stack each hold at most V elements.

### `bfs_order`

- Time: O(V + E)
- Space: O(V)
- Why: Every node is enqueued and dequeued once (V operations). For each node, all its edges are examined (E operations total). The queue and visited set each hold at most V elements.

### `dfs_order`

- Time: O(V + E)
- Space: O(V)
- Why: Same reasoning as BFS — each node is visited once and each edge is examined once. The stack and visited set each hold at most V elements.

### Stretch: `count_reachable_areas`

- Time: O(V + E)
- Space: O(V)
- Why: It calls `bfs_order` internally, which has O(V + E) time and O(V) space. Counting the length of the result is O(1) extra.

---

## Edge-Case Checklist

- [x] empty graph — all functions return `[]`, `False`, or `0` safely
- [x] missing start area — all functions check `if start not in graph` and return early
- [x] missing target area — `has_path` checks `if target not in graph` and returns `False`
- [x] `start == target` — `has_path` returns `True` as soon as the current node matches target (which happens on the first pop)
- [x] graph with a cycle — the `visited` set prevents infinite loops in all traversals
- [x] disconnected graph — BFS/DFS only visit nodes reachable from `start`; the locked wing is correctly excluded when starting from Lobby
- [x] area with no neighbors — the neighbor loop simply does nothing; the area is still added to the result

**Tricky edge case:** `start == target` for a missing area. For example, `has_path(graph, "Ballroom", "Ballroom")` must return `False`, not `True`. This is handled by checking `if start not in graph` before anything else.

---

## Tests Added

No new tests were added. All 18 existing tests pass, including the two stretch tests for `count_reachable_areas`.

---

## Known Limitations

No known limitations.

---

## Assistance & Sources

AI used? Y

AI helped with:
- explanations of BFS and DFS concepts
- understanding why `reversed()` is needed when pushing onto a DFS stack
- reviewing edge cases to make sure all scenarios were covered

Other sources used:

- Python `collections.deque` documentation: https://docs.python.org/3/library/collections.html#collections.deque
- Python `dict.get()` documentation: https://docs.python.org/3/library/stdtypes.html#dict.get
