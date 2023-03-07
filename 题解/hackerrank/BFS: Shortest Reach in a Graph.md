

```py
from typing import *
from collections import defaultdict, deque
# from sortedcontainers import SortedList


class Graph:
    # n: number of nodes
    def __init__(self, n: int):
        self.n = n
        self.adjList = defaultdict(list)
    def connect(self, node1: int, node2: int):
        self.adjList[node1].append(node2)
        self.adjList[node2].append(node1)
        
    def find_all_distances(self, startNode: int) -> List[int]:
        weight = 6
        queue = deque()
        queue.append([startNode, 0])
        visited = set()
        result = [-1] * self.n
        while queue:
            curNode, distance = queue.popleft()
            if curNode in visited:
                continue
            result[curNode] = distance * weight
            visited.add(curNode)
            
            nextNodes = self.adjList[curNode]
            for nextNode in nextNodes:
                queue.append([nextNode, distance + 1])
                    
        return [result[i] for i, num in enumerate(result) if i != startNode]
                    
        
                    
        
t = int(input())
for i in range(t):
    n,m = [int(value) for value in input().split()]
    graph = Graph(n)
    for i in range(m):
        x,y = [int(x) for x in input().split()]
        graph.connect(x-1,y-1) 
    s = int(input())
    result = graph.find_all_distances(s-1)
    print(' '.join(map(str, result)))
```