# 冒泡排序
### 代码实现

``` golang
func bubbling(arr []int) []int {
    length := len(arr)
	for range arr {
		for j := 1; j < length; j++ {
			if arr[j] < arr[j-1] {
				tmp := arr[j]
				arr[j] = arr[j-1]
				arr[j-1] = tmp
			}
		}
	}
	return arr
}
```
时间复杂度

$$
O(n^{2})
$$

### 优化
因为跟着循环，数组会从最后一个依次有序，所以有序部分不需再比较，跳过即可。
``` golang
func bubbling(arr []int) []int {
    length := len(arr)
	for i := 0; i < length; i++ {
		for j := 1; j < length - i; j++ {
			if arr[j] < arr[j-1] {
				tmp := arr[j]
				arr[j] = arr[j-1]
				arr[j-1] = tmp
			}
		}
	}

	return arr
}
```

时间复杂度

$$
O(n^{2})
$$