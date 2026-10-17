判断两个字典的键是否相同，有几种方法：

**方法1：直接比较 `.keys()`**
```python
d1 = {'a': 1, 'b': 2}
d2 = {'a': 999, 'b': 888}

print(d1.keys() == d2.keys())  # True
```

**方法2：转成 `set` 比较（推荐）**
```python
d1 = {'a': 1, 'b': 2}
d2 = {'a': 999, 'b': 888}

print(set(d1.keys()) == set(d2.keys()))  # True
# 或更简洁：
print(set(d1) == set(d2))  # True
```

**方法3：用 `sorted`（如果需要看顺序）**
```python
print(sorted(d1.keys()) == sorted(d2.keys()))  # True
```

**示例：**

```python
d1 = {'a': 1, 'b': 2, 'c': 3}
d2 = {'a': 999, 'b': 888, 'c': 777}
d3 = {'a': 1, 'b': 2}

print(d1.keys() == d2.keys())  # True - 键相同
print(d1.keys() == d3.keys())  # False - 键不同
```

**最简洁推荐：**
```python
set(d1) == set(d2)  # 只看键，不看值
```