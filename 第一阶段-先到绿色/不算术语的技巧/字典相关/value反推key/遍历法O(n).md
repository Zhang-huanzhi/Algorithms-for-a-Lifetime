在 Python 的标准库中，没有一种名为“反向字典”的内置数据结构可以直接通过 value 反推 key。Python 的内置 dict 是哈希表，它是为了 key → value 的 $O(1)$ 查询而设计的。

### 1. 遍历法（最直接，但慢）

如果你只是偶尔查一次，不需要额外的数据结构。

复杂度：$O(n)$

```python
my_dict = {'a': 1, 'b': 2, 'c': 1}
target_value = 1

# 找到第一个匹配的 key
key = next((k for k, v in my_dict.items() if v == target_value), None)

# 找到所有匹配的 key (因为 value 可能重复)
keys = [k for k, v in my_dict.items() if v == target_value]
```






---

### 