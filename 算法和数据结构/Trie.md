TrieNode三要素

- val
- is_word
- children

trie并不是用来查找单词, 而是用来匹配前缀的. 直接查找单词用set更为简单

python实现
- 通常find函数需要根据题目的具体情况进行修改

```python
class TrieNode:
    def __init__(self, val) -> None:
        self.val = val
        self.is_word = False
        self.children = {}

class Trie:
    def __init__(self) -> None:
        self.root = TrieNode("")
        self.root.is_word = True

    def add_word(self, word: str):
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode(c)

            cur = cur.children[c]

        cur.is_word = True

    def find(self, word: str) -> bool:
        cur = self.root
        for c in word:
            if c in cur.children:
                cur = cur.children[c]
            else:
                return False

        return cur.is_word

```