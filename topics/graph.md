# Graph Problems

## Evaluate Division
[Link](https://leetcode.com/problems/evaluate-division/)
```python
def calcEq(eqs, vals, queries):
  graph = {}

  def buildGraph(eqs, vals):
    for eq, val in zip(eqs, vals):
      A, B = eq
      graph[A] = graph.get(A, []) + [(B, val)]
      graph[B] = graph.get(B, []) + [(A, 1/val)]

  def findPath(query):
    A, B = query
    if A not in graph or B not in graph:
      return -1.0
    
    queue = collections.deque([(A, 1.0)])
    visited = set()

    while queue:
      a, accum_prod = queue.popleft()
      if a == B:
        return accum_prod
      
      visited.add(a)
      for neighbor, val in graph[a]:
        if neighbor not in visited:
          queue.append((neighbor, accum_prod*val))
    return -1.0

  buildGraph(eqs, vals)
  return [findPath(query) for query in queries]
```
Notes:
- We're searching for a "solution". There's various variables and associated values in route to a solution...sounds like a graph!
- Given a/b = 2 and b/c = 3
- (a -- 2 --> b -- 3 --> c)
- Same for backward (c -- 0.33 --> b -- 0.5 --> a)
- Build directed graph with these associations. For a query, travel down graph using BFS/DFS
- To construct a graph, it's just using a dictionary. To look through graph, loop through a node's neighbors (like the backtracking questions!!)