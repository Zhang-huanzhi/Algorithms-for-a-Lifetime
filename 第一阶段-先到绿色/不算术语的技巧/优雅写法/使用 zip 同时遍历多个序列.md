## 6. 使用 zip 同时遍历多个序列

当你需要同时处理两个列表（比如坐标 xx x 和 yy y）时。

### ❌ 不够优雅

python

```python
for i in range(len(list_a)):
    print(list_a[i], list_b[i])
```

### ✅ 优雅写法

python

```python
for a, b in zip(list_a, list_b):
    print(a, b)
```