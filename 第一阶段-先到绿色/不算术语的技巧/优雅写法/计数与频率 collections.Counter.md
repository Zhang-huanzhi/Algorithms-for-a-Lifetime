## 1. 计数与频率：collections.Counter

### ❌ 不够优雅

python

```python
d = {}
for x in arr:
    if x not in d: 
        d[x] = 0
    d[x] += 1
```

### ✅ 优雅写法

python

```python
from collections import Counter
counts = Counter(arr) 
# counts[x] 如果不存在会自动返回 0，不会报错
```