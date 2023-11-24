

线段树: 关注动态数据(动态区间)

应用范围
- 更新: 更新区间中一个元素或者一个区间的值
- 查询: 查询一个区间[i,j]的最大值, 最小值, 或者区间数字和


经典线段树问题: 区间染色

有一面墙, 长度为n, 每次选择一段墙进行染色. 
m次操作后, 我们可以看见多少种颜色?
m次操作后, 我们可以在[i,j]区间内看见多少种颜色?

染色操作(更新区间) 数组O(n)
查询操作(查询区间) 数组O(n)

另一类经典问题: 区间查询
查询一个区间[i,j]的最大值, 最小值, 或者区间数字和.
实质: 基于区间的统计查询

实例:
2017年注册用户中, 消费最高的用户? 消费最少的用户? 学习时间最长的用户?
某个太空区间中天体的总量?



线段树: 节点是区间的树
![](__attachment/f41a5ffd2a5911a7f92cba89cdd0aa26_MD5.png)





线段树不是完全二叉树, 是平衡二叉树




# 例题
- [[303. Range Sum Query - Immutable]]



```go
package main

import "errors"

type SegmentTree struct {
	data []int
	tree []int
}

func NewSegmentTree(data []int) *SegmentTree {
	tempData := make([]int, len(data))
	copy(tempData, data)

	st := &SegmentTree{
		data: data,
		tree: make([]int, 4*len(data)),
	}

	st.buildSegmentTree(0, 0, len(data)-1)
	return st
}

func (this *SegmentTree) buildSegmentTree(treeIndex, l, r int) {
	if l == r {
		this.tree[treeIndex] = this.data[l]
		return
	}

	leftTreeIndex, rightTreeIndex := this.leftChild(treeIndex), this.rightChild(treeIndex)
	mid := l + (r-l)/2
	this.buildSegmentTree(leftTreeIndex, l, mid)
	this.buildSegmentTree(rightTreeIndex, mid+1, r)

	this.tree[treeIndex] = this.merge(this.tree[leftTreeIndex], this.tree[rightTreeIndex])
}

func (this *SegmentTree) merge(a, b int) int {
	return a + b
}

func (this *SegmentTree) leftChild(i int) int {
	return i*2 + 1
}
func (this *SegmentTree) rightChild(i int) int {
	return i*2 + 2
}

func (this *SegmentTree) Query(l, r int) (int, error) {
	if l < 0 || l >= len(this.data) || r < 0 || r >= len(this.data || l > r) {
		return -1, errors.New("invalid scope")
	}
	return this.query(0, 0, len(this.data)-1, l, r), nil
}

func (this *SegmentTree) query(treeIndex, l, r, queryL, queryR int) int {
	if l == queryL && r == queryR {
		return this.tree[treeIndex]
	}

	mid := l + (r-l)/2
	leftTreeIndex, rightTreeIndex := this.leftChild(treeIndex), this.rightChild(treeIndex)

	if queryL >= mid+1 {
		return this.query(rightTreeIndex, mid+1, r, queryL, queryR)
	} else if queryR <= mid {
		return this.query(leftTreeIndex, l, mid, queryL, queryR)
	}

	return this.merge(
		this.query(leftTreeIndex, l, mid, queryL, mid),
		this.query(rightTreeIndex, mid+1, r, mid+1, queryR),
	)
}

func (this *SegmentTree) Set(index, num int) {
	this.set(0, 0, len(this.data)-1, index, num)
}

func (this *SegmentTree) set(treeIndex, l, r, index, num int) {
	if l == r {
		this.tree[treeIndex] = num
		return
	}

	mid := l + (r-l)/2
	leftTreeIndex, rightTreeIndex := treeIndex*2+1, treeIndex*2+2
	if index >= mid+1 {
		this.set(rightTreeIndex, mid+1, r, index, num)
	} else {
		this.set(leftTreeIndex, l, mid+1, index, num)
	}

	this.tree[treeIndex] = this.merge(this.tree[leftTreeIndex], this.tree[rightTreeIndex])
}

```

test
```go
package main

import (
	"fmt"
	"testing"

	"github.com/stretchr/testify/assert"
)

func TestSegmentTree(t *testing.T) {
	assert := assert.New(t)
	nums := []int{-2, 0, 3, -5, 2, -1}
	st := NewSegmentTree(nums)

	fmt.Println(st.tree)

	result, err := st.Query(0, 2)
	assert.NoError(err)
	assert.Equal(1, result)
	result, err = st.Query(2, 5)
	assert.NoError(err)
	assert.Equal(-1, result)
	result, err = st.Query(0, 5)
	assert.NoError(err)
	assert.Equal(-3, result)
}

```