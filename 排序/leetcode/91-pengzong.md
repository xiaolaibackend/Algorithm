# 91.解码方法
### 题目
一条包含字母 A-Z 的消息通过以下映射进行了 编码 ：
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

要 解码 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，"11106" 可以映射为：

* "AAJF" ，将消息分组为 (1 1 10 6)
* "KJF" ，将消息分组为 (11 10 6)

注意，消息不能分组为  (1 11 06) ，因为 "06" 不能映射为 "F" ，这是由于 "6" 和 "06" 在映射中并不等价。

给你一个只含数字的 非空 字符串 s ，请计算并返回 解码 方法的 总数 。

题目数据保证答案肯定是一个 32 位 的整数。


### 思路
因为映射是从1-26，所以每次只考虑字符串的前两个字符即可，有两种可能（先不考虑边界情况）：

* s[1:]的情况
* 当s0*10+s1<=26时，s[2:]的情况

将字符串问题转化为子串的问题（递归）
``` golang
// 递归代码
func numDecodings(s string) int {
    if len(s) < 1 {
		return 0
	}

	oneNum,_ := strconv.Atoi(string(s[0]))
	if oneNum == 0 {
		return 0
	}

	if len(s) == 1 {
		return 1
	}

	s = s[1:]
	one := numDecodings(s)

	two := 0
	if len(s) >= 1 {
		twoNum,_ := strconv.Atoi(string(s[0]))
		if oneNum * 10 + twoNum <= 26 {
			two = 1
			if len(s) > 1 {
				s = s[1:]
				two = numDecodings(s)
			}
		}
	}

	return one + two
}
```
进一步考虑性能问题，因为递归会遇到大量重复遍历相同子串的问题，所以只要将相同子串的结果用map存下来，下次直接从map取出来就行了。

#### 优化后代码：
``` golang
// 存储已经遍历过的结果
var answers = make(map[int]int)

func numDecodings(s string) int {
	answers = make(map[int]int)
	return numDecodings2(s)
}

func numDecodings2(s string) int {
	length := len(s)
	if num,ok := answers[length]; ok {
		return num
	}

	if length < 1 {
		answers[length] = 0
		return 0
	}

	oneNum,_ := strconv.Atoi(string(s[0]))
	if oneNum == 0 {
		answers[length] = 0
		return 0
	}

	if length == 1 {
		answers[length] = 1
		return 1
	}

	s = s[1:]
	one := numDecodings2(s)

	two := 0
	if len(s) >= 1 {
		twoNum,_ := strconv.Atoi(string(s[0]))
		if oneNum * 10 + twoNum <= 26 {
			two = 1
			if len(s) > 1 {
				s = s[1:]
				two = numDecodings2(s)
			}
		}
	}

	answers[length] = one + two
	return one + two
}
```
