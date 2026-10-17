## 2. 列表推导式（List Comprehension）

这是 Python 的灵魂，能在一行内完成"过滤"和"转换"。

### ❌ 不够优雅

python

```python
res = []
for x in range(10):
    if x % 2 == 0:
        res.append(x * x)
```

### ✅ 优雅写法

python

```python
res = [x * x for x in range(10) if x % 2 == 0]
```