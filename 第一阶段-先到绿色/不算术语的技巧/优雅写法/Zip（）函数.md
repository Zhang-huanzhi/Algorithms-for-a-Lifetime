## 第二步：zip() 函数 (The Zipper)

**场景：** 同时遍历两个或多个长度相同的列表

**例子：** 你有"决策列表"和"数据列表"

```python
chances = [1, 0, 1]
items = ["苹果", "香蕉", "橘子"]

for chosen, name in zip(chances, items):
    if chosen:
        print(f"你买了: {name}")
```