SortedList 不是 Python 的标准库，需要通过 pip install sortedcontainers 安装。在 AtCoder 平台中，该库默认是预装好的，可以直接 from sortedcontainers import SortedList 导入使用。

存入 SortedList 的元素必须是**可比较大小**的（例如不能把 str 和 int 混合存入同一个 SortedList 中）。

虽然 `SortedList` 支持像普通列表一样通过 `sl[i]` 获取元素，但请记住这个操作的复杂度是 $\mathcal{O}(\log N)$ 而非 $\mathcal{O}(1)$。在循环中大量、随机地通过索引访问元素时需要注意这一开销。

底层用的是 **分块列表（List of Lists)**

---

| 容器           | 有序  | 找左右邻居         | 插入效率         |
| ------------ | --- | ------------- | ------------ |
| `list`       | 否   | 需排序或遍历        | `O(N)`       |
| `set`        | 否   | 不方便           | 平均 `O(1)`    |
| `SortedList` | 是   | `O(log N)` 定位 | 约 `O(log N)` |


| 操作         | 对应方法                  | 时间复杂度                 | 备注                                                            |
| :--------- | :-------------------- | :-------------------- | :------------------------------------------------------------ |
| **创建**     | `sl = SortedList()`   | $\mathcal{O}(1)$      | 也可以用已有序列初始化，复杂度 $\mathcal{O}(N \log N)$                       |
| **添加元素**   | `sl.add(value)`       | $\mathcal{O}(\log N)$ | 自动插入到正确位置并保持有序                                                |
| **删除指定值**  | `sl.remove(value)`    | $\mathcal{O}(\log N)$ | 若值不存在会抛出 KeyError (可用 `discard` 避免)                           |
| **位置二分查找** | `sl.bisect_left(val)` | $\mathcal{O}(\log N)$ | 寻找插入位置的索引                                                     |
| **按索引访问**  | `sl[i]`               | $\mathcal{O}(\log N)$ | **注意**：不像普通列表是 $\mathcal{O}(1)$，但 $\mathcal{O}(\log N)$ 依然非常快 |
| **按索引弹出**  | `sl.pop(i)`           | $\mathcal{O}(\log N)$ | 默认弹出最后一个元素                                                    |
| **获取大小**   | `len(sl)`             | $\mathcal{O}(1)$      | 直接读取内部维护的计数器                                                  |


它特别适合动态维护“当前有哪些数，以及某个数左右两侧分别是谁”的题。

```
from sortedcontainers import SortedList

sl = SortedList([5, 1, 3])
print(sl)  # SortedList([1, 3, 5])

sl.add(4)
print(sl)  # SortedList([1, 3, 4, 5])
```

常用操作：

```
sl = SortedList([1, 3, 5, 5, 8])

sl.add(4)                 # 插入一个元素，保持有序
sl.remove(5)              # 删除一个 5；不存在会报错
sl.discard(100)           # 删除一个 100；不存在也不报错

p1 = sl.bisect_left(5)    # 第一个 >= 5 的位置
p2 = sl.bisect_right(5)   # 第一个 > 5 的位置

print(sl[p1])             # 5
print(sl[0], sl[-1])      # 最小值、最大值
```

`bisect_left(x)` 与 `bisect_right(x)` 的区别在于重复元素：

```
sl = SortedList([1, 3, 5, 5, 5, 8])

print(sl.bisect_left(5))   # 2，第一个 5 的位置
print(sl.bisect_right(5))  # 5，最后一个 5 的后一位
```

在本题中，插入 `y` 前：

```
pos = points.bisect_left(y)
left = points[pos - 1]                    # 左邻居
right = points[pos] if pos < len(points) else None  # 右邻居
```

