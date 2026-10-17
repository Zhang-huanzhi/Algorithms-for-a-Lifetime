## 8. enumerate()：同时获取索引和数值

不需要再写 `range(len(arr))`。

### ❌ 不够优雅

python

```python
for i in range(len(arr)):
    x = arr[i]
    print(f"第{i}个元素是{x}")
```

### ✅ 优雅写法

python

```python
for i, x in enumerate(arr):
    print(f"第{i}个元素是{x}")
```