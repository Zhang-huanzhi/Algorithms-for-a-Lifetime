AtCoder 官方提供了名为 **ACL（AtCoder Library）** 的算法库，其 Python 版本在 AtCoder 评测机中是**默认内置**的（可以通过 `from atcoder.dsu import DSU` 直接导入）。

它内部实现了 **“路径压缩 + 按大小合并（Union by Size）”** 双重优化，单次操作的平摊时间复杂度为极其高效的 $O(\alpha(N))$（几乎等于 $O(1)$）。

下面为你详细介绍它的所有方法与使用技巧。

---

### 一、 API 方法清单

`DSU` 类一共提供了 **1 个构造函数 + 5 个核心方法**：

| 方法 | 功能描述 | 返回值 | 时间复杂度 |
| :--- | :--- | :--- | :--- |
| `DSU(n)` | **初始化**一个包含 $n$ 个元素（编号为 $0 \sim n-1$）的并查集 | 新的 `DSU` 对象 | $O(N)$ |
| `merge(a, b)` | 将包含点 $a$ 和点 $b$ 的两个集合**合并** | 合并后该集合的**代表元（根节点）** | 平摊 $O(\alpha(N))$ |
| `same(a, b)` | 查询点 $a$ 与点 $b$ **是否在同一个集合**中 | `True` 或 `False` | 平摊 $O(\alpha(N))$ |
| `leader(a)` | 查询点 $a$ 所在集合的**代表元（根节点）** | `int`（根节点编号） | 平摊 $O(\alpha(N))$ |
| `size(a)` | 查询点 $a$ 所在集合的**元素总数（集合大小）** | `int`（集合内节点数） | 平摊 $O(\alpha(N))$ |
| `groups()` | 获取当前图中的**所有连通块（连通分量）** | `list[list[int]]` | $O(N)$ |

---

### 二、 方法详细解析与代码演示

#### 1. 初始化 `DSU(n)`
- 默认创建 $n$ 个独立的连通分量，编号从 **$0$ 到 $n - 1$**。

```python
from atcoder.dsu import DSU

dsu = DSU(5)  # 创建包含 0, 1, 2, 3, 4 的并查集
```

#### 2. `merge(a, b)` —— 合并
- 如果点 $a$ 和点 $b$ 本就属于同一集合，它什么都不做，直接返回代表元。
- 如果属于不同集合，它会按照小集合并入大集合的规则合并两者，并**返回合并后新集合的根节点**。

```python
root1 = dsu.merge(0, 1)  # 连边 0-1
root2 = dsu.merge(2, 3)  # 连边 2-3
```

#### 3. `same(a, b)` —— 连通性查询
- 判断 $a$ 和 $b$ 能否互相到达。

```python
print(dsu.same(0, 1))  # 输出 True
print(dsu.same(0, 2))  # 输出 False
```

#### 4. `size(a)` —— 连通块大小（极其好用）
- 题目经常会问“包含某点的网络一共有多少个节点”，无需自己额外开数组维护 `size`，直接调用即可。

```python
dsu.merge(0, 2)  # 现在 0, 1, 2, 3 连成了同一个大连通块
print(dsu.size(0))  # 输出 4 (包含 0, 1, 2, 3)
print(dsu.size(4))  # 输出 1 (4 还是孤立点)
```

#### 5. `leader(a)` —— 查找根节点
- 相当于传统写法中的 `find(a)`，返回集合的唯一代表元。通常用于哈希、给连通块去重染色等。

```python
print(dsu.leader(1) == dsu.leader(3))  # True，因为已经在同一个大集合中
```

#### 6. `groups()` —— 一键获取所有连通分量（特色王牌功能）
- 这是 ACL 最受好评的功能之一！它能以 $O(N)$ 的极快速度，把所有的连通块按列表分好类返回。

```python
# 此时集合情况：{0, 1, 2, 3} 和 {4}
print(dsu.groups())
# 输出可能是：[[0, 1, 2, 3], [4]]
```

---

### 三、 常见技巧与踩坑指南

#### 1. 0-based 与 1-based 索引陷阱（最容易错的地方）
AtCoder 的很多题目节点编号是从 **$1$ 到 $N$**，而 ACL 的 `DSU` 是严格 **$0$ 到 $N-1$**。

**有两种处理姿势：**

* **姿势 A（最不易出错）：初始化多开一个位置**
  ```python
  n, m = map(int, input().split())
  dsu = DSU(n + 1)  # 开大小为 n + 1，此时支持下标 0 ~ n
  # 之后读入 1-based 的 u, v 可以直接传入，无须任何转换：
  dsu.merge(u, v)
  ```
  *(注意：此时 `dsu.groups()` 会把闲置的 `[0]` 作为一个多余集合算进去，如果用到 `groups` 记得忽略或过滤掉含 0 的组)*

* **姿势 B（规范严谨）：读入时转换成 0-based**
  ```python
  dsu = DSU(n)
  u, v = map(int, input().split())
  dsu.merge(u - 1, v - 1)  # 手动减 1
  ```

#### 2. 能否在本地运行？
如果在自己电脑的本地 Python 环境中运行，默认是没有这个库的，执行 `from atcoder.dsu import DSU` 会报 `ModuleNotFoundError`。
- 本地安装方法：终端运行 `pip install ac-library-python` 即可与 AtCoder 线上环境完全一致。