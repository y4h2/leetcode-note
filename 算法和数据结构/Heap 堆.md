
Heap的特点: 关注动态极值


heapify: 把一个array转换成heap
从倒数第一个非叶子节点(n-1)/2, 从后往前遍历, 然后直接shiftDown. 时间复杂度为O(n)
通过insert来构建heap的时间复杂度为O(nlogn)



heap限定大小的意义
在N个元素中选出前M个元素
使用quick sort, 时间复杂度是O(nlogn)
用大小为M的heap, 时间复杂度是O(nlogM)


## go语言实现

int

```go
package main

import (
	"errors"
)

type Heap struct {
	items []int
}

func NewHeap() *Heap {
	return &Heap{
		items: []int{},
	}
}

// i, 2 * i + 1, 2 * i + 2

func (this *Heap) Insert(item int) {
	this.items = append(this.items, item)

	for i := len(this.items) - 1; i >= 1 && !this.Less(i, (i-1)/2); i = (i - 1) / 2 {
		this.Swap(i, (i-1)/2)
	}
}

func (this *Heap) Less(i, j int) bool {
	return this.items[i] < this.items[j]
}

func (this *Heap) Swap(i, j int) {
	this.items[i], this.items[j] = this.items[j], this.items[i]
}

func (this *Heap) Pop() (int, error) {
	n := len(this.items)
	if n == 0 {
		return -1, errors.New("invalid")
	}
	top := this.items[0]
	this.items[0] = this.items[n-1]
	this.items = this.items[:n-1]

	this.Heapify(0)
	return top, nil
}

func (this *Heap) Heapify(index int) {
	i := index
	n := len(this.items)
	for {
		maxPos := i
		if i*2+1 < n && this.Less(i, i*2+1) {
			maxPos = i*2 + 1
		}
		if i*2+2 < n && this.Less(maxPos, i*2+2) {
			maxPos = i*2 + 2
		}
		if maxPos == i {
			break
		}

		this.Swap(i, maxPos)
		i = maxPos
	}
}

func (this *Heap) Size() int {
	return len(this.items)
}

func (this *Heap) Sort() {
	n := len(this.items) - 1

	for n >= 0 {
		this.Swap(0, n)
		n--
		this.Heapify(0)
	}
}

```


struct
```go
package main

import (
	"errors"
)

type Item struct {
	count int
	num   int
}

type Heap struct {
	items []Item
}

func NewHeap() *Heap {
	return &Heap{
		items: []Item{},
	}
}

func (this *Heap) Push(item Item) {
	this.items = append(this.items, item)

	this.ShiftUp(len(this.items) - 1)
}

func (this *Heap) Pop() (Item, error) {
	n := len(this.items)
	if n == 0 {
		return Item{}, errors.New("not found")
	}

	result := this.items[0]
	this.items[0] = this.items[n-1]
	this.items = this.items[:n-1]
	this.ShiftDown(0)

	return result, nil
}

func (this *Heap) ShiftUp(index int) {
	for i := index; i != (i-1)/2 && !this.Cmp((i-1)/2, i); i = (i - 1) / 2 {
		this.Swap(i, (i-1)/2)
	}
}

func (this *Heap) ShiftDown(i int) {
	n := len(this.items)
	for {
		maxPos, leftPos, rightPos := i, i*2+1, i*2+2
		if leftPos < n && !this.Cmp(i, leftPos) {
			maxPos = leftPos
		}
		if rightPos < n && !this.Cmp(maxPos, rightPos) {
			maxPos = rightPos
		}

		if maxPos == i {
			return
		}

		this.Swap(i, maxPos)
		i = maxPos
	}
}

func (this *Heap) Cmp(i, j int) bool {
	return this.items[i].count > this.items[j].count
}

func (this *Heap) Swap(i, j int) {
	this.items[i], this.items[j] = this.items[j], this.items[i]
}

func topKFrequent(nums []int, k int) []int {
	hash := map[int]int{}
	for _, num := range nums {
		hash[num]++
	}

	heap := NewHeap()
	for k, v := range hash {
		heap.Push(Item{
			count: v,
			num:   k,
		})
	}

	result := make([]int, k)
	for i := 0; i < k; i++ {
		item, _ := heap.Pop()
		result[i] = item.num
	}

	return result
}

```
test

```go
package main

import (
	"testing"

	"github.com/stretchr/testify/assert"
)

func TestTopK(t *testing.T) {
	assert := assert.New(t)

	assert.Equal([]int{0}, topKFrequent([]int{3, 0, 1, 0, 1, 0}, 1))
	assert.Equal([]int{1, 2}, topKFrequent([]int{1, 1, 1, 2, 2, 3}, 2))
}

```



例题
- [[1792. Maximum Average Pass Ratio]]
- [[2335. Minimum Amount of Time to Fill Cups]]
k路合并
- [[23. Merge k Sorted Lists]]