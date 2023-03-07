---
tags: 排除法
---

核心思想: 从所有pair中, 除掉good pair, 剩下的即为bad pair

直接求解: TLE
即遍历所有的pair, 如果pair满足bad pair则result + 1
```py
class Solution:
    def countBadPairs(self, nums: List[int]) -> int:
        n = len(nums)
        result = 0
        for i in range(n):
            for j in range(i, n):
                if nums[i] - nums[j] != i - j:
                    result += 1
        
        return result
```


排除法: 

转换思路: 正向求解超时, good pair. (bad pair属于零散解, 需要一个一个的统计, good pair则存在规律, 位置和差值有关)
一个数组如果全是good pair, 则为[1,2,3,4,5]的类型 ([-1,0,1,2,3]属于相同的解), 不同的值不好统计(我们不好统计1,2,3,4,5这种序列有多长), 但是可以把他们转换成相同的值, 即[num1 - 1, num2 - 2, num3 - 3, num4 - 4, num5 - 5], 这样good pair的值一定相同. 算出有多少good pair即可
```py
class Solution:
    def countBadPairs(self, nums: List[int]) -> int:
        n = len(nums)
        increaseArr = [i+1 for i in range(n)]
        for i in range(n):
            nums[i] -= increaseArr[i]
        
        countMap = defaultdict(int)
        for num in nums:
            countMap[num] += 1
            
        result = n * (n + 1) // 2
        for k, count in countMap.items():
            result -= count * (count + 1) // 2
            
        return result
```