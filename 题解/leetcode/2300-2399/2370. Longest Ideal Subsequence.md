---

tags: dp/字母
---



核心思想: 对字符串的dp降维



DP暴力解法: TLE
n = len(s)
i in [0, n-1]: dp[i]表示以s[i]这个字符串为结尾的ideal string的最长长度
递推公式为: 
- dp[i] = max(for j in range[0:i-1] if condition) + 1, dp[i])
- condition: s[j] is in k range from s[i]

结果超时: 没有完全利用题目条件
- 时间复杂度O(n^2)
- 要遍历前面所有的dp[j], 但是由于题目限制所有的值都在26个字母中, ideal的条件也是由字母间的距离计算的, 所以我们可以直接存储所有的字母的最长序列长度即可 (贪心思想, 如果有两个同样以字符c结尾的字符串, s[i]必然优先连接更长的字符串)
```py
class Solution:
    def longestIdealString(self, s: str, k: int) -> int:
        n = len(s)
        if n == 1:
            return 1
        dp = [1] * n
        dp[0] = 1
        
        for i in range(1, n):
            for j in range(0, i):
                if abs(ord(s[i]) - ord(s[j])) <= k:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp)
```



DP: 26个字母解法
c in [0, 25]: dp[c]表示第c个字母, 在字符串s[0:i-1]中的最长长度
递推公式为:
- dp[i] = max(for j in range[0:25] if condition) + 1, dp[i])
- condition: 0:25中的char is in k range from s[i]


```py
class Solution:
    def longestIdealString(self, s: str, k: int) -> int:
        n = len(s)
        if n == 1:
            return 1
        
        hashmap = Hashmap()
        
        hashmap.set(s[0], 1) # the first char
        for i in range(1, n):
            # update the records when i continues
            maxLength = hashmap.maxAvailableLength(s[i], k)
            
            hashmap.set(s[i], maxLength + 1)
        return hashmap.max()
    
# store the maximum ideal string length of each char
class Hashmap:
    def __init__(self):
        self.hashmap = {}
        for i in range(26):
            self.hashmap[i] = 0
    
    def max(self):
        return max(self.hashmap[i] for i in range(26))
            
            
    # find all available characters which is within range k from c
    # each character has its max ideal string length stored in hashmap
    # return the max value from them
    def maxAvailableLength(self, c: str, k: int) -> int:
        ordC = ord(c) - ord('a')
        return max(self.hashmap[i] for i in range(max(0,  ordC - k), min(25, ordC + k) + 1))
            
    
    def get(self, c: str) -> int:
        return self.hashmap[ord(c) - ord('a')]
    
    def set(self, c: str, val: int):
        self.hashmap[ord(c) - ord('a')] = val
```