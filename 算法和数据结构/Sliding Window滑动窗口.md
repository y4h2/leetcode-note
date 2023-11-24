
滑动窗口特征
- 在滑动的过程中, 可以复用之前的结果
- 窗口的长度固定


最好把滑动窗口变动的部分封装成class, 比如[[30. Substring with Concatenation of All Words]]
```python

class Seen:
    def __init__(self, word_map: dict, expected: int) -> None:
        self.map = collections.defaultdict(int)
        self.word_map = word_map
        self.count = 0
        self.expected = expected

    def add(self, word):
        if word not in self.word_map:
            return
        if self.map[word] < self.word_map[word]:
            self.count += 1
        self.map[word] += 1

    def remove(self, word):
        if word not in self.word_map:
            return
        self.map[word] -= 1
        if self.map[word] < self.word_map[word]:
            self.count -= 1

    def is_solution(self) -> bool:
        return self.count == self.expected
        
```

基本接口add, remove ,is_solution