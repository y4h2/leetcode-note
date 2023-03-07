---
tags: dp
---

n = len(nums)
i in [0, n]: dp[i]表示nums[0:i-1]是否valid
递推公式:
- dp[i] = dp[i-2] and (nums[i-1:i-2] satisfy condition1) or dp[i-3] and (nums[i-1:i-3] satisfy condition2 or condition3)
- condition1: The subarray consists of exactly 2 equal elements.
- condition2: The subarray consists of exactly 3 equal elements.
- condition3: The subarray consists of exactly 3 consecutive increasing elements, that is, the difference between adjacent elements is 1



```py
class Solution:
    def validPartition(self, nums: List[int]) -> bool:
        n = len(nums)
        dp = [False] * (n + 1)
        dp[0] = True
        dp[1] = False
        dp[2] = (nums[0] == nums[1])
        
        
        for i in range(3, n+1):
            index = i - 1
            # 满足题目给出的条件
            dp[i] = (dp[i-2] and nums[index] == nums[index-1]) or (dp[i-3] and ((nums[index] == nums[index-1] and (nums[index] == nums[index-2])) or ((nums[index] == nums[index-1] +1) and (nums[index] == nums[index-2] + 2))))
        
        
        return dp[n]
```