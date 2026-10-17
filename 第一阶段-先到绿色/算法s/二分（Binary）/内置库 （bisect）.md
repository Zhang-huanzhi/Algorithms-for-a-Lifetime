**bisect** = **bi**nary **sect**ion（二分分割）

- **bi-** = 二（binary）
- **-sect** = 分割（section/cut）
- 意思是：通过**二分法**来分割/查找数组

在 ABC 比赛中，如果你需要在一个**已经排好序的数组**里找某个位置，千万不要手写二分，直接用 Python 自带的库，速度快且不容易写错。

``` python
import bisect

arr = [1, 3, 3, 3, 5, 7, 9]

# 1. 找到“第一个大于等于 x”的索引 (Lower Bound)
idx1 = bisect.bisect_left(arr, 3) 
# 结果是 1 (arr[1] 是第一个 3)

# 2. 找到“第一个大于 x”的索引 (Upper Bound)
idx2 = bisect.bisect_right(arr, 3) 
# 结果是 4 (arr[4] 是 5)

# 3. 统计数组中有多少个 x
count = bisect.bisect_right(arr, 3) - bisect.bisect_left(arr, 3)
```