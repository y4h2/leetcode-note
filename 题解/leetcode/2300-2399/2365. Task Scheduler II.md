

- 任务按给定的顺序执行
- 每个任务受前一个同种任务影响, 所以只要记录前一个同种任务在什么时候执行的即可



```py
class Solution:
    def taskSchedulerII(self, tasks: List[int], space: int) -> int:
        prevMap = {}
        
        result = 0
        for task in tasks:
            prevMap[task] = -space
        
        n = len(tasks)
        for i in range(n):
            task = tasks[i]
            result += 1
            if result - prevMap[task] <= space:
                result = prevMap[task] + space + 1
            
            prevMap[task] = result
            # print(result)
            
        return result
```