算法中的 key，就是把“题目的排序规则”编码成一个可比较的值；多关键字排序时，就把规则按优先级写成元组。


---

### **基础模板**

| 需求 | 代码 | 说明 |
|------|------|------|
| 升序排序 | `arr.sort()` | 默认升序 |
| 降序排序 | `arr.sort(reverse=True)` | 全体降序 |
| 按第 i 项升序 | `arr.sort(key=lambda x: x[i])` | 元组/列表的某一项 |
| 按第 i 项降序 | `arr.sort(key=lambda x: -x[i])` | 数值取负 |

### **多关键字模板**

| 需求 | 代码 |
|------|------|
| 先按 a 升序，再按 b 升序 | `arr.sort(key=lambda x: (x.a, x.b))` |
| 先按 a 升序，再按 b 降序 | `arr.sort(key=lambda x: (x.a, -x.b))` |
| 先按 a 降序，再按 b 升序 | `arr.sort(key=lambda x: (-x.a, x.b))` |
| 先按 a 降序，再按 b 降序 | `arr.sort(key=lambda x: (-x.a, -x.b))` |

### **竞赛实战模板**

**模板 1：带编号的三关键字排序**
```python
n = int(input())
arr = []
for i in range(n):
    a, b = map(int, input().split())
    arr.append((a, b, i+1))  # 保存编号

arr.sort(key=lambda x: (x[0], -x[1], x[2]))  # a升序，b降序，编号升序

for item in arr:
    print(item[2])  # 输出编号
```

**模板 2：字符串多关键字排序**
```python
arr = []
for i in range(n):
    s, score = input().split()
    arr.append((s, int(score), i+1))

arr.sort(key=lambda x: (x[0], -x[1], x[2]))  # 名字升序，分数降序，编号升序
```

**模板 3：区间贪心排序**
```python
intervals = []
for _ in range(n):
    l, r = map(int, input().split())
    intervals.append((l, r))

intervals.sort(key=lambda x: x[1])  # 按右端点排序（最多区间数）
# 或
intervals.sort(key=lambda x: (x[0], x[1]))  # 按左端点再按右端点（区间合并）
```

**模板 4：数对排序**
```python
arr = [(a, b) for a, b in zip(arr_a, arr_b)]
arr.sort(key=lambda x: (x[0], -x[1]))
```

### **关键记忆点**

| 要点 | 记忆 |
|------|------|
| **升序** | 直接写 |
| **降序** | 数值取负 `-` |
| **多关键字** | 用元组 `(第一关键字, 第二关键字, ...)` |
| **保留原序** | 加上编号作为最后一个关键字 |
| **字符串降序** | 不能取负，考虑分两次排或用 `functools.cmp_to_key` |

### **debug 检查清单**

做排序题时检查这几点：

- [ ] 是否需要保存原编号？
- [ ] 每个关键字是升序还是降序？
- [ ] 数值降序是否取了负号？
- [ ] 相同情况下的处理规则是否完整？
- [ ] 排序后是直接输出还是继续处理？

---

**一句话**：看到排序题，先在草稿纸上写清楚"第一关键字、第二关键字、升降序"，然后翻译成 `key=lambda x: (条件1, 条件2)`。