# 后言

### 1. 颜色去重逻辑存在漏洞
在您的第一段循环中，有这样一行代码：
```python
if temp != c:
    cnt += 1
    temp = c
    visited[i] = True
    ans += value
```
因为您的 `arr` 是**按价值从大到小排序的，而不是按颜色排序的**。
这意味着相同颜色的宝石在数组中可能是不相邻的。例如，如果排序后的宝石颜色序列是 `[1, 2, 1, 3]`：
* 处理到第 3 个宝石（颜色为 1）时，此时 `temp` 的值为上一次记录的 `2`。
* 因为 `temp != c`（即 `2 != 1`），代码会认为这是一种“新颜色”，并将其计入 `cnt`。但实际上颜色 `1` 在第 1 个宝石时就已经被选过了。这导致您无法准确统计出是否真的选够了 $M$ 种不同的颜色。

# Code
``` python
import sys

def main():

    n, k, m = map(int, sys.stdin.readline().strip().split())

    arr = []

    for i in range(n):
        arr.append(tuple(map(int ,sys.stdin.readline().strip().split())))

    arr.sort(key = lambda x:(-x[1], x[0]))

    ans = 0
    temp = 0
    cnt = 0
    visited = [False] * (n + 1)

    for i in range(n):
        c, value = arr[i][0], arr[i][1]

        if cnt == m:
            break
            
        if temp != c:
            cnt += 1
            temp = c
            visited[i] = True
            ans += value



    cnt = 0
    for i in range(n):
        if cnt == k - m:
            break
            
        if not visited[i]:
            ans += arr[i][1]
            cnt += 1

    print(ans)


if __name__ == '__main__':
    main()
```
# 补充图片

![[Pasted image 20260607130356.png]]