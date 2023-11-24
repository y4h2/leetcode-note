

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

模板
```py
lo, hi = 0, n
while lo + 1e-6 < hi:
    mid = (lo + hi) / 2

    # do something
    result = ...
    if result <= target:
        hi = mid
    else:
        lo = mid

return hi
```

例题
774. Minimize Max Distance to Gas Station
mid表示加油站之间的最小距离， 原始加油站之间的最大距离为上限，0为下限，target是k。 每次根据mid，求至少需要多少新加油站才能达到mid.



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





## 代码模板

```go
package main

import (
	"errors"
	"testing"
)

func BinarySearch(arr []int, target int) (int, error) {
	low, high := 0, len(arr)-1
	for low <= high {
		mid := low + (high-low)>>1
		if target == arr[mid] {
			return mid, nil
		} else if target < arr[mid] {
			high = mid - 1
		} else {
			low = mid + 1
		}
	}

	return -1, errors.New("not found")
}

func TestBinarySearch(t *testing.T) {
	t.Log("Given an array")
	arr := []int{1, 2, 3, 5, 8, 10, 20, 100}

	index, err := BinarySearch(arr, arr[5])
	assert := NewAssert(t)
	assert.NoError(err)
	assert.Equal(5, index)
}

// 找到第一个值等于给定值的元素
func BinarySearchFindFirst(arr []int, target int) (int, error) {
	low, high := 0, len(arr)-1
	for low <= high {
		mid := low + (high-low)>>1
		if arr[mid] > target {
			high = mid - 1
		} else if arr[mid] < target {
			low = mid + 1
		} else {
			if mid == 0 || arr[mid-1] != target {
				return mid, nil
			} else {
				high = mid - 1
			}
		}
	}

	return -1, errors.New("not found")
}

// 查找最后一个值等于给定值的元素
func BinarySearchFindLast(arr []int, target int) (int, error) {
	low, high := 0, len(arr)-1
	for low <= high {
		mid := low + (high-low)>>1
		if arr[mid] < target {
			low = mid + 1
		} else if arr[mid] > target {
			high = mid - 1
		} else {
			if mid == len(arr)-1 || arr[mid+1] != target {
				return mid, nil
			} else {
				low = mid + 1
			}
		}
	}

	return -1, errors.New("not found")
}

// 查找第一个大于等于给定值的元素
func BinarySearchFirstGE(arr []int, target int) (int, error) {
	low, high := 0, len(arr)-1

	for low <= high {
		mid := low + (high-low)>>1
		if arr[mid] >= target {
			if mid == 0 || !(arr[mid-1] >= target) {
				return mid, nil
			} else {
				high = mid - 1
			}
		} else {
			low = mid + 1
		}
	}

	return -1, errors.New("not found")
}

// 查找最后一个小于等于给定值的元素
func BinarySearchLastLE(arr []int, target int) (int, error) {
	low, high := 0, len(arr)-1

	for low <= high {
		mid := low + (high-low)>>1
		if arr[mid] <= target {
			if mid == len(arr)-1 || !(arr[mid+1] <= target) {
				return mid, nil
			} else {
				low = mid + 1
			}
		} else {
			high = mid - 1
		}
	}

	return -1, errors.New("not found")
}

type Assert struct {
	t *testing.T
}

func NewAssert(t *testing.T) *Assert {
	return &Assert{t}
}

func (a *Assert) NoError(err error) {
	if err != nil {
		a.t.Errorf("expect no error but got %s", err.Error())
	}
}

func (a *Assert) Equal(i, j int) {
	if i != j {
		a.t.Errorf("expect %d, got %d", i, j)
	}
}

func matchString(a, b string) (bool, error) {
	return a == b, nil
}

func main() {
	testSuite := []testing.InternalTest{
		{
			Name: "test case 1",
			F:    TestBinarySearch,
		},
	}
	testing.Main(matchString, testSuite, nil, nil)
}

```


python代码

```python

from typing import *

def binarySearchFirstEqual(nums: List[int], target: int) -> int:
  n = len(nums)
  l, r = 0, n-1
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


print(binarySearchFirstEqual([1, 2, 3, 3, 3, 3, 4, 5, 6, 7], 3) == 2)
print(binarySearchLastEqual([1, 2, 3, 3, 3, 3, 4, 5, 6, 7], 3) == 5)
print(binarySearchFirstGE([1, 2, 3, 3, 3, 3, 4, 5, 6, 7], 3) == 2)
print(binarySearchFirstGE([1, 2, 3, 3, 3, 3, 4, 5, 6, 7], 4) == 6)
print(binarySearchLastLE([1, 2, 3, 3, 3, 3, 4, 5, 6, 7], 3) == 5)
print(binarySearchLastLE([1, 2, 3, 3, 3, 3, 4, 5, 6, 7], 4) == 6)

```