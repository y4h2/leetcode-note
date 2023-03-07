

暴力解即可

```py
class Solution:
    def arithmeticTriplets(self, nums: List[int], diff: int) -> int:
        result = 0
        n = len(nums)
        hashmap = {}
        
        for num in nums:
            hashmap[num] = True
        
        for i in range(n):
            if (nums[i] + diff) in hashmap and (nums[i] + 2 * diff) in hashmap:
                result += 1
                
        return result
```