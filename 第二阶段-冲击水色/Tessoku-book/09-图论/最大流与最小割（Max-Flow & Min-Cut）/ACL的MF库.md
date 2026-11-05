在官方库中，最大流模块指的是 **`atcoder.maxflow.MFGraph`**。

它不仅仅能计算一个最大流水位数值，还提供了一整套查询**边流量、最小割集合、修改边**的完整 API。

---

### 一、完整方法一览表

| 方法名 | 作用 | 返回值 |
| :--- | :--- | :--- |
| **`MFGraph(n)`** | 初始化一个有 $n$ 个顶点的网络流图（编号从 $0$ 到 $n-1$） | 图对象 |
| **`add_edge(u, v, cap)`** | 添加一条从 $u$ 到 $v$，容量为 $cap$ 的有向边 | 返回这条边的**唯一编号（0, 1, 2...）** |
| **`flow(s, t)`** | 计算从源点 $s$ 到汇点 $t$ 的**最大流** | 最大流量（整数） |
| **`flow(s, t, flow_limit)`** | 计算最大流，但限制**最大推流上限**不超过 `flow_limit` | 流量（整数） |
| **`min_cut(s)`** | 跑完最大流后调用，获取**最小割**的点集划分 | 长度为 $n$ 的 `bool` 列表 |
| **`get_edge(i)`** | 查询第 $i$ 条边当前的实际流向信息 | 一个边对象（包含起点、终点、容量、**实际流量**） |
| **`edges()`** | 查询所有加入的边的当前状态 | 包含所有边对象的列表 |
| **`change_edge(i, new_cap, new_flow)`** | 动态修改第 $i$ 条边的容量和当前流量 | `None` |

---

### 二、核心方法详解与代码实战

#### 1. 初始化与加边：`MFGraph(n)` & `add_edge(u, v, cap)`
* **注意**：节点编号是 **$0$-indexed**（$0$ 到 $n-1$）。
* `add_edge` 会返回该边的索引（从 0 开始自增），方便后续精准查询这条边。

```python
from atcoder.maxflow import MFGraph

# 创建一个 4 个点 (0, 1, 2, 3) 的图
g = MFGraph(4)

# 0 -> 1，容量为 5；返回 0（代表第 0 条边）
edge_id = g.add_edge(0, 1, 5) 
g.add_edge(1, 3, 3)
g.add_edge(0, 2, 4)
g.add_edge(2, 3, 4)
```

---

#### 2. 求最大流：`flow(s, t)` 与流量限制
```python
# 计算从源点 0 到汇点 3 的最大流
max_flow = g.flow(0, 3)
print(max_flow)  # 输出实际的最大流量

# 可选：如果只想看能不能流满 5 升水（限流）
# g.flow(0, 3, flow_limit=5)
```

---

#### 3. 求最小割：`min_cut(s)`（输出方案的关键！）
**非常重要**：在解决上一题（B68 - 选车站）时，如果题目要求**输出到底选了哪些车站**，就必须用这个方法。
* 它返回一个布尔数组 `res`，长度为 $N$。
* `res[i] == True`：代表在残量网络中，源点 $s$ 能走到点 $i$（即点 $i$ 属于源点集合 $S$）。
* `res[i] == False`：代表点 $i$ 属于汇点集合 $T$。

```python
# 必须先跑 g.flow(s, t)，再调 min_cut
g.flow(0, 3)
cut = g.min_cut(0)

# cut 是形如 [True, False, True, False] 的列表
for node_id in range(4):
    if cut[node_id]:
        print(f"节点 {node_id} 属于源点集合 S")
    else:
        print(f"节点 {node_id} 属于汇点集合 T")
```

---

#### 4. 查看具体哪条边流了多少：`get_edge(i)` 和 `edges()`
常用于**二分图匹配**（求谁和谁配对）、或者**打印水流具体路径**。

返回的边对象包含以下属性：
* `.src` 或 `from_`：边的起点
* `.dst` 或 `to`：边的终点
* `.cap`：边的初始容量
* **`.flow`：这条边最终实际流过了多少水！**

```python
# 获取第 0 条边的运行情况
e = g.get_edge(0)
print(f"边从 {e.src} -> {e.dst}，总容量: {e.cap}，实际流过: {e.flow}")

# 遍历所有边，打印所有被水流填满的管道（满流边）
for i, e in enumerate(g.edges()):
    if e.flow > 0:
        print(f"边 {i}: 从 {e.src} 流向 {e.dst}，流量为 {e.flow}")
```

---

### 三、综合应用案例（二分图最大匹配 + 输出配对方案）

比如有 2 个工人、2 个任务：
* 工人 1 可以做任务 1、任务 2
* 工人 2 可以做任务 2
* 求最大能匹配几对，并**输出谁配了谁**：

```python
from atcoder.maxflow import MFGraph

# 点编号：
# 源点: 0
# 工人 1, 2: 编号 1, 2
# 任务 1, 2: 编号 3, 4
# 汇点: 5
g = MFGraph(6)
S, T = 0, 5

# S 连工人，容量 1
g.add_edge(S, 1, 1)
g.add_edge(S, 2, 1)

# 工人连任务（记录这些边的 ID，方便后续查看匹配结果）
edge1 = g.add_edge(1, 3, 1) # 工人 1 -> 任务 1
edge2 = g.add_edge(1, 4, 1) # 工人 1 -> 任务 2
edge3 = g.add_edge(2, 4, 1) # 工人 2 -> 任务 2

# 任务连 T，容量 1
g.add_edge(3, T, 1)
g.add_edge(4, T, 1)

# 1. 求最大匹配数
max_matching = g.flow(S, T)
print(f"最多匹配数: {max_matching}")

# 2. 查看具体的配对情况（看中间哪条边的 flow == 1）
for edge_id in [edge1, edge2, edge3]:
    e = g.get_edge(edge_id)
    if e.flow == 1:
        worker = e.src
        task = e.dst - 2  # 还原任务编号
        print(f"工人 {worker} 匹配了 任务 {task}")
```