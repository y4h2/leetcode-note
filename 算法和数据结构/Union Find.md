普通算法和压缩算法



```go
package main

type UnionFind struct {
	parent []int
}

func NewUnionFind(size int) *UnionFind {
	parent := make([]int, size)
	for i := range parent {
		parent[i] = i
	}

	return &UnionFind{parent: parent}
}

func (this *UnionFind) Find(p int) int {
	for p != this.parent[p] {
		p = this.parent[p]
	}
	return p
}

func (this *UnionFind) Union(p, q int) {
	pRoot, qRoot := this.Find(p), this.Find(q)
	if pRoot == qRoot {
		return
	}

	this.parent[pRoot] = qRoot
}

func (this *UnionFind) IsConnected(p, q int) bool {
	return this.Find(p) == this.Find(q)
}

type CompressUnionFind struct {
	UnionFind
}

func (this *CompressUnionFind) Find(p int) int {
	for p != this.parent[p] {
		this.parent[p] = this.parent[this.parent[p]]
		p = this.parent[p]
	}

	return p
}

```