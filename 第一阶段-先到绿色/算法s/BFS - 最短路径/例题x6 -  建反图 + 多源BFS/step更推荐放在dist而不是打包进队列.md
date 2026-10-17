结论是：**在大多数算法题中，确实更推荐把 `step`（距离）存进数组（如 `dist[x][y]`）里，而不是每次都打包进队列 `(x, y, step)`。**

但你的写法并不是“错”，而是 **“可行但不够极致”**。我们来看看两者的对比以及各自适用的场景：

---

### 一、 为什么通常不把 `step` 放进队列？

#### 1. 消除数据冗余
在绝大多数 BFS 中，我们本来就需要一个 `visited` 数组来防止重复访问。
* **做法 A（你的做法）**：用 `visited` 标记访问，同时在队列存 `(x, y, step)`。
  * 相当于 `visited` 和 `step` 各自占了内存，信息存了两遍。
* **做法 B（更优做法）**：把 `visited` 初始化为 `-1`，直接用来当 `dist`（距离数组）。
  * 未访问：`dist[x][y] == -1`
  * 已访问且步数为 $d$：`dist[x][y] = d`
  * **一个二维数组既当了 visited，又存了 step，一举两得**。

#### 2. 显著提升性能（特别是 Python）
Python 是一门解释型语言，对象的创建和解包开销很大：
* 队列里存 `(x, y, step)`：每扩散一个点，就要在内存中**新建一个 3 元素元组**，出队时又要**解包 3 个变量**。
* 如果网格是 $5 \times 10^5$，相当于频繁创建、销毁几十万个元组对象，会明显拖慢运行速度，甚至带来垃圾回收（GC）的额外耗时。

---

### 二、 标准写法长什么样？（两种最常用的范式）

#### 范式 1：利用 `dist` 数组记录步数（最通用、最推荐）
```python
# dist 既是 visited，也是 step 记录器
dist = [[-1] * w for _ in range(h)]
q = deque()

# 初始化起点
dist[start_x][start_y] = 0
q.append((start_x, start_y))  # 只存坐标

while q:
    x, y = q.popleft()
    step = dist[x][y]  # 直接查表拿 step
    
    for nx, ny in neighbors(x, y):
        if dist[nx][ny] == -1:       # 既判断了未访问
            dist[nx][ny] = step + 1  # 又更新了距离
            q.append((nx, ny))
```

#### 范式 2：按层遍历（无需任何 step 变量，直接利用队列长度）
如果你只关心“一层一层往外走”，可以每次按当前队列长度 `len(q)` 批量出队。**这种写法队列里不仅不用存 step，连 dist 数组都不用记数字，直接用 bool visited 就行**：

```python
q = deque([(start_x, start_y)])
visited = [[False] * w for _ in range(h)]
visited[start_x][start_y] = True

step = 0
while q:
    if step == k:  # 已经走了 k 步，直接停下！
        break
        
    # 一次性处理完当前这一层（当前步数）的所有节点
    for _ in range(len(q)):
        x, y = q.popleft()
        for nx, ny in neighbors(x, y):
            if not visited[nx][ny]:
                visited[nx][ny] = True
                q.append((nx, ny))
                
    step += 1  # 这一层处理完，步数 +1
```

---

### 三、 什么时候**必须**把状态放进队列？

并不是说队列里绝对不能存额外信息，在以下场景下，队列**必须存额外状态**：

1. **分层图 / 状态压缩 BFS**：
   * 比如题目要求：“可以消除最多 $K$ 个障碍物”、“身上带了某几把钥匙”。
   * 此时状态不再仅仅是坐标 $(x, y)$，而是 $(x, y, \text{剩余消除次数})$ 或 $(x, y, \text{钥匙掩码 mask})$。
   * 这时队列必须存元组 `(x, y, remain_k)`。

---

### 总结
你的原代码能想到用 `(x, y, step)` 已经非常棒了（完全体现了正确的 BFS 思维）。在以后的简单最短路题目中，顺手把它改成 **“用 `dist` 数组替代 `step`”** 或 **“按层 `len(q)` 遍历”**，你的代码就会在**内存、常数速度和优雅度**上更上一层楼！