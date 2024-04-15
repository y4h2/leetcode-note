# 基本二分法

关键点

- 变量定义一致: l, r = 0, n-1. 则while循环的运行条件是l <= r, l和r的范围内始终保持有效的nums
- 循环内对于符合条件的处理和对于不符合条件的处理

核心在于如果部分满足条件, 则需要对条件进行拆分


二分法的多种模板
要检查最后是否收敛

最后收敛一定是lo == hi
检查是否有死循环，lo = 0, hi = 1带入，mid = 0
第一个分支，hi = 0, lo == hi 结束，不会有死循环
第二个分支，lo = 1, lo == hi 结束，不会有死循环
```py
lo, hi = 0, math.inf
while lo < hi:
    mid = lo + (hi - lo) // 2
    if checkOk(mid):
        hi = mid
    else:
        lo = mid + 1
```



## 找到第一个值等于target的元素

找到值相等, 但是我们需要进一步判断是否是**第一个**值

```python
def binarySearchFirstEqual(nums: List[int], target: int) -> int:
  n = len(nums)
  l, r = 0, n - 1
  while l <= r:
    mid = (l + r) >> 1
    if nums[mid] > target:
      r = mid - 1
    elif nums[mid] < target:
      l = mid + 1
    else: # equal
      if mid == 0 or nums[mid-1] != target:
        return mid
      else:
        r = mid - 1

  return -1
```

## 找到最后一个值等于给定值的元素

```python
def binarySearchLastEqual(nums: List[int], target: int) -> int:
  n = len(nums)
  l, r = 0, n - 1
  while l <= r:
    mid = (l + r) >> 1
    if nums[mid] < target:
      l = mid + 1
    elif nums[mid] > target:
      r = mid - 1
    else:
      if mid == n - 1 or nums[mid+1] != target:
        return mid
      else:
        l = mid + 1
  
  return -1
```

## 查找第一个大于等于给定值的元素

```python
def binarySearchFirstGE(nums: List[int], target: int) -> int:
  n = len(nums)
  l, r = 0, n - 1
  while l <= r:
    mid = (l + r) >> 1
    if nums[mid] < target:
      l = mid + 1
    elif nums[mid] >= target:
      if mid == 0 or not(nums[mid-1] >= target):
        return mid
      else:
        r = mid - 1

  return -1
```

## 查找最后一个小于等于给定值的元素

```python
def binarySearchLastLE(nums: List[int], target: int) -> int:
  n = len(nums)
  l, r = 0, n - 1
  while l <= r:
    mid = (l + r) >> 1
    if nums[mid] > target:
      r = mid - 1
    elif nums[mid] <= target:
      if mid == n - 1 or not(nums[mid+1] <= target):
        return mid
      else:
        l = mid + 1

  return -1
```

# 二分法的一般应用
bisect.bisect_left: 第一个大于等于的元素的index，包含当前元素， bisect.bisect_left([1,2,3,3,4], 3) => 2, bisect.bisect_left([1,2,3,3,4], 2.5) => 2
bisect.bisect_right: 第一个大于的元素的index, 不包含当前元素， bisect.bisect_right([1,2,3,3,4], 3) => 4, bisect.bisect_right([1,2,3,3,4], 2.5) => 2


- [[2055. Plates Between Candles]]: bisect_right - bisect_left
```python
class Solution:
    def platesBetweenCandles(self, s: str, queries: List[List[int]]) -> List[int]:
        candlePos = [i for i, c in enumerate(s) if c == '|']        
        result = []
        
        for start, end in queries:
            l = bisect.bisect_left(candlePos, start)
            r = bisect.bisect(candlePos, end) - 1
            
            # candlePos[r] - candlePos[l] + 1 : length between two candles
            # r - l + 1: number of candles
            result.append(candlePos[r] - candlePos[l] - (r - l) if l < r else 0)
            
        return result

```



# 二分法的高级应用

二分搜索的套路比较固定，如果遇到一个算法问题，能够确定 x, f(x), target 分别是什么，并写出单调函数 f 的代码。


- [[875. Koko Eating Bananas]]
这题珂珂吃香蕉的速度就是自变量 x，吃完所有香蕉所需的时间就是单调函数 f(x)，target 就是吃香蕉的时间限制 H。

- [[2226. Maximum Candies Allocated to K Children]]
每个人能分到的糖果是x, 按x能分配给多少人是单调函数f(x), target就是最少需要分给k个人。（f(x) >= k）

- [[1802. Maximum Value at a Given Index in a Bounded Array]]
x: nums[index]能取的最大值， f(x)以nums[index]为最大值，sum(nums)能取的最小值， target： maxSum


- [[1539. Kth Missing Positive Number]]

- [[1482. Minimum Number of Days to Make m Bouquets]]
x: day, f(x)第mid天，能做多少bouquet, target: m


- [1283. Find the Smallest Divisor Given a Threshold]()

- [1231. Divide Chocolate]()
x: sweetness, f(x)能分多少块比x大的，target: k+1

- [1011. Capacity To Ship Packages Within D Days]()
x: capacity, f(x)基于x要装多少天， target: D

- [[410. Split Array Largest Sum]]
x: max sum of all subarray, f(x)基于x 需要生成多少个subarray, target: k

- [[400. Nth Digit]]
浮点数二分法

[[774. Minimize Max Distance to Gas Station]]
mid表示加油站之间的最小距离， 原始加油站之间的最大距离为上限，0为下限，target是k。 每次根据mid，求至少需要多少新加油站才能达到mid.


