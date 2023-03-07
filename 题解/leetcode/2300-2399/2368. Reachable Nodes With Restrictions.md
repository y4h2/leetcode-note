---
tags: graph/dfs
---


保险起见直接按有环图的遍历处理
- 构造邻接表: 无向图
- 遍历碰到restricted则停止

```py
class Solution:
    def reachableNodes(self, n: int, edges: List[List[int]], restricted: List[int]) -> int:
        self.result = {}
        self.restricted = {}
        self.visited = {}
        for node in restricted:
            self.restricted[node] = True
            
        self.edges = defaultdict(list)
        
        for edge in edges:
            src, dst = edge    
            self.edges[src].append(dst)
            self.edges[dst].append(src)
        
        self.traverse(0)
        
        return len(self.result.keys())
        
    def traverse(self, node: int):
        self.visited[node] = True
        if node in self.restricted:
            return
        
        self.result[node] = True
        for nextNode in self.edges[node]:
            if nextNode not in self.visited:
                self.traverse(nextNode)
            
```