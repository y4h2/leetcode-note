

```py
#!/bin/python3

import math
import os
import random
import re
import sys
import heapq

#
# Complete the 'runningMedian' function below.
#
# The function is expected to return a DOUBLE_ARRAY.
# The function accepts INTEGER_ARRAY a as parameter.
#

def runningMedian(nums):
    # Write your code here
    curSum = 0
    heap = []
    result = []
    minHeap = []
    maxHeap = []
    
    for num in nums:
        if len(minHeap) == len(maxHeap):
            heapq.heappush(minHeap, num)
            heapq.heappush(maxHeap, -heapq.heappop(minHeap))
        else: # len(maxHeap) > len(minHeap)
            heapq.heappush(maxHeap, -num)
            heapq.heappush(minHeap, -heapq.heappop(maxHeap))
            
        if len(minHeap) == len(maxHeap):
            result.append((minHeap[0] - maxHeap[0]) / 2)
        else:
            result.append(-maxHeap[0])

        
    return [round(num, 1) for num in result]

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    a_count = int(input().strip())

    a = []

    for _ in range(a_count):
        a_item = int(input().strip())
        a.append(a_item)

    result = runningMedian(a)

    fptr.write('\n'.join(map(str, result)))
    fptr.write('\n')

    fptr.close()

```