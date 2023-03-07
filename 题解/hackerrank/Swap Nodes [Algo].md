
https://www.hackerrank.com/challenges/swap-nodes-algo/problem

```py
#!/bin/python3

import math
import os
import random
import re
import sys
from collections import deque

#
# Complete the 'swapNodes' function below.
#
# The function is expected to return a 2D_INTEGER_ARRAY.
# The function accepts following parameters:
#  1. 2D_INTEGER_ARRAY indexes
#  2. INTEGER_ARRAY queries
#

class TreeNode:
    def __init__(self, data, left=None, right=None):
        self.data = data
        self.left = left
        self.right = right

def inorder(root):
    result = []
    stack = deque([root])
    visited = set()
    while stack:
        node = stack.pop()
        if node is None:
            continue
        if node.data in visited:
            result.append(node.data)
            continue
        visited.add(node.data)
        stack.append(node.right)
        stack.append(node)
        stack.append(node.left)
    return result

def swap(root, k):
    queue = deque([(root, 1)])
    while queue:
        node, depth = queue.popleft()
        if not node:
            continue
        if depth % k == 0:
            node.left, node.right = node.right, node.left
        queue.append((node.left, depth+1))
        queue.append((node.right, depth+1))

def swapNodes(indexes, queries):
    # Write your code here
    n = len(indexes)
    nodes = [None] * (n + 1)
    nodes[1] = TreeNode(1)
    for i in range(1, n+1):
        nodes[i] = TreeNode(i)
    for i in range(n):
        left, right= indexes[i]
        if left != -1:
            nodes[i+1].left = nodes[left]
        if right != -1:
            nodes[i+1].right = nodes[right]
    
    root = nodes[1]
    # print(inorder(root))
    result = []
    for query in queries:
        swap(root, query)
        result.append(inorder(root))
        
    return result

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    n = int(input().strip())

    indexes = []

    for _ in range(n):
        indexes.append(list(map(int, input().rstrip().split())))

    queries_count = int(input().strip())

    queries = []

    for _ in range(queries_count):
        queries_item = int(input().strip())
        queries.append(queries_item)

    result = swapNodes(indexes, queries)

    fptr.write('\n'.join([' '.join(map(str, x)) for x in result]))
    fptr.write('\n')

    fptr.close()

```