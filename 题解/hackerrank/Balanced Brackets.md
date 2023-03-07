

```py
#!/bin/python3

import math
import os
import random
import re
import sys

#
# Complete the 'isBalanced' function below.
#
# The function is expected to return a STRING.
# The function accepts STRING s as parameter.
#

def isBalanced(s):
    if helper(s):
        return 'YES'
    else:
        return 'NO'
    
def helper(s):
    # Write your code here
    stack = []
    
    bracketMap = {
        '}': '{',
        ')': '(',
        ']': '[',
    }
    
    for c in s:
        if c in '{[(':
            stack.append(c)
        else:
            print(stack, c)
            if stack and bracketMap[c] == stack[-1]:
                stack.pop(-1)
            else:
                return False
            
    return len(stack) == 0

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    t = int(input().strip())

    for t_itr in range(t):
        s = input()

        result = isBalanced(s)

        fptr.write(result + '\n')

    fptr.close()

```