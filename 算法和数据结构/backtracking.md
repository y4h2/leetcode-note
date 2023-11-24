
backtracking的核心问题
- 什么时候用for循环
- 什么时候回退 
- 什么时候剪枝 (用if)

核心思想: 用递归树来模拟backtracking算法
- 每调用一次backtracking函数, 都是往下在移动一层
- 每次回退一次, 就是向上返回一层
- 每次剪枝都是用if去掉了一些不成立的情况, 避免浪费时间递归


回退的本质就是恢复到递归树的当前层


所谓剪枝, 就是选择是否应该进入下一层
[[22. Generate Parentheses]] 是一个非常适合学习剪枝的例题
```python
def backtracking(self, left_bracket: int, right_bracket: int):
	if left_bracket == self.n and right_bracket == self.n:
		self.result.append(''.join(self.track))
		return

	if left_bracket < self.n:
		self.track.append('(')
		self.backtracking(left_bracket + 1, right_bracket)
		self.track.pop(-1)

	if right_bracket < left_bracket:
		self.track.append(')')
		self.backtracking(left_bracket, right_bracket + 1)
		self.track.pop(-1)
```
在进入下一层之前避免不合法的情况出现



深入理解回溯
回溯通常和剪枝结合


[[51. N-Queens]]
[[2305. Fair Distribution of Cookies]]





# 排列 组合 子集
https://labuladong.github.io/algo/1/8/

回溯解法

# 组合问题

两种风格

1. 所有backtrack公用一个数组 （由于递归并没有并发的情况，所以是可行的，只是要记得回退）
2. 每次调用backtrack传入一个新的数组

风格1

```python
class Solution:
    def run(self, candidates: List[int]) -> List[List[int]]:
        self.track = []
        self.result = []
        self.candidates = candidates

        self.backtrack(0)

        return self.result 

    def backtrack(self, index: int):
        # 满足条件，加入result
        self.result.append(self.track[:])

        for i in range(index, len(self.candidates)):
            self.track.append(self.candidates[index])
            # 根据不同的题型 传入不同的值进行递归
            self.backtrack(i)
            self.track.pop(-1)
```

风格2

```python
class Solution:
    def run(self, candidates: List[int]) -> List[List[int]]:
        self.result = []
        self.candidates = candidates

        self.backtrack(0)

        return self.result 

    def backtrack(self, curArr: List[int], index: int):
        # 满足条件，加入result
        self.result.append(curArr[:])

        for i in range(index, len(self.candidates)):
            tempArr = curArr[:]
            tempArr.append(self.candidates[i])
            # 根据不同的题型 传入不同的值进行递归
            self.backtrack(tempArr, i)
```

# 排列

排列问题由于需要选不同的值放在不同的位置，所以需要一个visited数组

```python
class Solution:
    def run(self, candidates: List[int]) -> List[List[int]]:
        self.track = []
        self.result = []
        self.candidates = candidates
        self.visited = [False] * len(self.candidates)

        self.backtrack(0)

        return self.result 

    def backtrack(self):
        # 满足条件，加入result
        self.result.append(self.track[:])

        for i in range(0, len(self.candidates)):
            if self.visited[i]:
                continue
            self.visited[i] = True
            self.track.append(self.candidates[i])
        
            self.backtrack(i)
            self.track.pop(-1)
            self.visited[i] = False
```

# subset

subset包含combination

# 剪枝

去重的问题本质上还是剪枝

通过递归树来理解剪枝的问题


[[77. Combinations]]
[[39. Combination Sum]]
[[40. Combination Sum II]]
[[216. Combination Sum III]]
[[46. Permutations]]
[[47. Permutations II]]
[[78. Subsets]]
