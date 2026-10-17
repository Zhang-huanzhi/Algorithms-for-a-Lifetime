## 5. any() 和 all()：条件判断大杀器

判断列表里是否有一个满足条件，或全部满足条件。

### ❌ 不够优雅

python

```python
has_even = False
for x in arr:
    if x % 2 == 0:
        has_even = True
        break
```

### ✅ 优雅写法

python

```python
# 是否存在偶数
if any(x % 2 == 0 for x in arr):
    pass

# 是否全是偶数
if all(x % 2 == 0 for x in arr):
    pass
```