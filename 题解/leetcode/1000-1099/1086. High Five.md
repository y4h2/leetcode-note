---
tags: 水题
---

```go
func highFive(items [][]int) [][]int {
    sort.Slice(items, func(i, j int) bool {
        if items[i][0] == items[j][0] {
            return items[i][1] > items[j][1]
        }
        
        return items[i][0] < items[j][0]
    })
    
    result := [][]int{}
    
    for i := 0; i < len(items); i++ {
        if i > 0 && items[i][0] == items[i - 1][0] {
            continue
        }
        sum := 0
        for j := 0; j < 5; j++ {
            sum += items[i+j][1]
        }
        
        result = append(result, []int{items[i][0], sum / 5})
    }
    
    return result
}
```