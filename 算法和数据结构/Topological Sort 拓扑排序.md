

```python
from typing import *
from collections import defaultdict, deque


# node start from 1
class Graph:
  def __init__(self, n: int, edges: List[List[int]]):
    self.n = n
    self.adj = defaultdict(list)
    for edge in edges:
      start, end = edge
      self.adj[start].append(end)

  def topologicalSort(self) -> List[int]:
    indegrees = [0] * self.n
    for k in self.adj.keys():
      for endNode in self.adj[k]:
        indegrees[endNode] += 1
    queue = deque([i + 1 for i in range(self.n) if indegrees[i] == 0])

    result = []
    while queue:
      cur = queue.popleft()
      result.append(cur)
      for v in self.adj[cur]:
        indegrees[v - 1] -= 1
        if indegrees[v - 1] == 0:
          queue.append(v)
    
    if len(result) == self.n:
      return result
    return None




```