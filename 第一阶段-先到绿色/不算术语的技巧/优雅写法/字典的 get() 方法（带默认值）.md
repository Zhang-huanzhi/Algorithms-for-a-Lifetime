## 7. 字典的 get() 方法（带默认值）

在统计或查表时，避免键不存在导致崩溃。

### ❌ 不够优雅

python

```python
if key in my_dict:
    val = my_dict[key]
else:
    val = 0
```

### ✅ 优雅写法

python

```python
val = my_dict.get(key, 0)  # 找不到就给 0
```