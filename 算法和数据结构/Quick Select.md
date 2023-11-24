[http://www.noteanddata.com/classic-algorithm-quick-select.html](http://www.noteanddata.com/classic-algorithm-quick-select.html)

1. 基本思路和快速排序类似 [http://www.noteanddata.com/classic-algorithm-quick-sort.html](http://www.noteanddata.com/classic-algorithm-quick-sort.html)， 也是一个不断partition的过程
2. 主要的区别是，每次partition完以后，只需要在pivot的一边继续递归就可以了，另外一边不需要处理可以直接扔掉。
3. 这里写的partition实现和原来的一样，都是嵌套循环，把i和j移动都符合条件的index


```go
package main

import "math/rand"

func sortColors(nums []int) {
	n := len(nums)
	i, j := -1, n
	cur := 0
	for cur < j {
		if nums[cur] == 0 {
			i++
			nums[i], nums[cur] = nums[cur], nums[i]
			cur++
		} else if nums[cur] == 1 {
			cur++
		} else { //nums[cur] == 2
			j--
			nums[cur], nums[j] = nums[j], nums[cur]
		}
	}
}

func quickSelect(nums []int, k int) int {
	i := rand.Intn(len(nums))

	start, _ := threeWaysSort(nums, nums[i])

	if k-1 == start {
		return nums[start]
	} else if k-1 > start {
		return quickSelect(nums[:start], k)
	} else {
		return quickSelect(nums[start:], k-start)
	}
}

// [i+1, j-1] is the equal region
func threeWaysSort(nums []int, target int) (start int, end int) {
	n := len(nums)
	i, j := -1, n
	cur := 0
	for cur < j {
		if nums[cur] == target {
			cur++
		} else if nums[cur] < target {
			i++
			nums[cur], nums[i] = nums[i], nums[cur]
			cur++
		} else { // nums[cur] > target
			j--
			nums[cur], nums[j] = nums[j], nums[cur]
		}
	}

	return i + 1, j - 1
}

```