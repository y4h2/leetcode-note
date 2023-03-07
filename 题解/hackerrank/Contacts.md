
https://www.hackerrank.com/challenges/contacts/problem

```py
#!/bin/python3

import math
import os
import random
import re
import sys

#
# Complete the 'contacts' function below.
#
# The function is expected to return an INTEGER_ARRAY.
# The function accepts 2D_STRING_ARRAY queries as parameter.
#

class TrieNode:
    def __init__(self, val):
        self.val = val
        self.children = {}
        self.count = 0
        
class Trie:
    def __init__(self):
        self.root= TrieNode('')
    def add(self, word):
        # self.root.count += 1
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode(c)
            cur = cur.children[c]
            cur.count += 1
    
    def find(self, prefix):
        cur = self.root
        for c in prefix:
            if c not in cur.children:
                return 0
            cur = cur.children[c]
            
        return cur.count        

def contacts(queries):
    trie = Trie()
    result = []
    # Write your code here
    for query in queries:
        command, s = query
        if command == 'add':
            trie.add(s)
        if command == 'find':
            result.append(trie.find(s))
            
    return result
    
    

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    queries_rows = int(input().strip())

    queries = []

    for _ in range(queries_rows):
        queries.append(input().rstrip().split())

    result = contacts(queries)

    fptr.write('\n'.join(map(str, result)))
    fptr.write('\n')

    fptr.close()

```