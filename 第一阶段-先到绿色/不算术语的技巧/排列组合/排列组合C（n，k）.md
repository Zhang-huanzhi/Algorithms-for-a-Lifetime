第一路：调用 Python 内置神器 `itertools`
第二路：手写递归 (DFS) 的通用模版
第三路：进阶——面对“天文数字”怎么办？-优先队列 (`heapq`)，动态规划 (`DP`)
``` python
import itertools

items = ['A', 'B', 'C']

# 1. 排列 (Permutations): 考虑顺序，不能重复选
# 结果: ('A','B'), ('A','C'), ('B','A'), ('B','C'), ('C','A'), ('C','B')
list(itertools.permutations(items, 2))

# 2. 组合 (Combinations): 不考虑顺序，不能重复选
# 结果: ('A','B'), ('A','C'), ('B','C')
list(itertools.combinations(items, 2))

# 3. 可重组合 (Combinations with replacement): 本题类型，不考虑顺序，能重复选
# 结果: ('A','A'), ('A','B'), ('A','C'), ('B','B'), ('B','C'), ('C','C')
list(itertools.combinations_with_replacement(items, 2))

# 4. 笛卡尔积 (Product): 相当于多层嵌套循环
# 结果: ('A','A'), ('A','B')... (类似两个集合乘积)
list(itertools.product(items, repeat=2))
```

---

