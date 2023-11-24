TrieNode三要素
- val
- is_word
- children


trie并不是用来查找单词, 而是用来匹配前缀的. 直接查找单词用set更为简单


python实现
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









```python
package main

import (
	"container/list"
	"strings"
)

type Node struct {
	r        rune
	isWord   bool
	children map[rune]*Node
}

func NewNode(r rune) *Node {
	return &Node{
		r:        r,
		children: map[rune]*Node{},
		isWord:   false,
	}
}

type Trie struct {
	root *Node
}

/** Initialize your data structure here. */
func Constructor() Trie {
	return Trie{
		root: NewNode('0'),
	}
}

/** Inserts a word into the trie. */
func (this *Trie) Insert(word string) {
	cur := this.root
	for _, r := range word {
		if _, found := cur.children[r]; !found {
			cur.children[r] = NewNode(r)
		}
		cur = cur.children[r]
	}

	cur.isWord = true
}

/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
	cur := this.root
	for _, r := range word {
		if _, found := cur.children[r]; !found {
			return false
		}
		cur = cur.children[r]
	}

	return cur.isWord
}

/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
	cur := this.root
	for _, r := range prefix {
		if _, found := cur.children[r]; !found {
			return false
		}
		cur = cur.children[r]
	}

	if cur.isWord {
		return true
	}

	return len(cur.children) > 0
}

/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */

// return first k matched bookmarks
// keep the origin order
func MatchK(bookmarks []string, prefix string, k int) []string {
	trie := NewTrie()
	for i, bookmark := range bookmarks {
		words := strings.Split(bookmark, " ")
		for _, word := range words {
			trie.Add(word, i)
		}
	}

	cur := trie.root
	for _, r := range prefix {
		if _, found := cur.children[r]; found {
			cur = cur.children[r]
		} else {
			return []string{}
		}
	}

	queue := list.New()
	for len(cur.children) != 0 {
		for r, next := range cur.children {

		}
	}

	return []string{}
}

```