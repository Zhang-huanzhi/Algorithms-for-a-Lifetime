``` python
import sys  
  
def solve():  
  
    h, w, n = map(int, sys.stdin.readline().strip().split())  
  
    arr = [ [0] * w for _ in range(h)]  
  
    for i in range(n):  
        x, y = map(int, sys.stdin.readline().strip().split())  
  
        arr[x-1][y-1] = 1  
  
    q = int(sys.stdin.readline().strip())  
  
    for _ in range(q):  
        line = sys.stdin.readline().strip().split()  
  
        op = line[0]  
  
        if op == '1':  
            x = int(line[1]) - 1  
            cnt = 0  
            for i in range(w):  
                if arr[x][i] == 1:  
                    cnt += 1  
                    arr[x][i] = 0  
            print(cnt)  
  
        if op == '2':  
            y = int(line[1]) - 1  
            cnt = 0  
            for i in range(h):  
                if arr[i][y] == 1:  
                    cnt += 1  
                    arr[i][y] = 0  
            print(cnt)  
  
if __name__ == '__main__':  
    solve()
```

---
这道题考察的是**“对数据结构的选择”**。

* **原代码的思维**：我是“上帝视角”，我画了一个巨大的地图，谁走哪我都知道（**稠密存储**）。
* **优化代码的思维**：我是“垃圾回收员”，我只关心哪里有垃圾，没垃圾的地方我连看都不看（**稀疏存储**）。

**核心结论：**
在竞赛编程中，**如果一个二维数组的行数和列数达到 $10^5$ 量级，除非它是 $O(1)$ 或 $O(N)$ 的空间利用，否则绝对不能直接开二维数组。** 必须通过哈希表、集合、链表等“稀疏化”手段，只处理那些“有效”的数据点。


---

原先代码中，导致性能崩塌的“多余操作”主要集中在以下三个方面，它们本质上都是在**浪费资源去处理“空无一物”的区域**：

### 1. 遍历“不存在的垃圾”（最核心的浪费）
在你的 `for` 循环中：
```python
for i in range(w):
    if arr[x][i] == 1: # 即使这行大部分格子都是 0，你依然在执行判断
        cnt += 1
        arr[x][i] = 0
```
* **问题：** 如果这一行有 20 万个格子，但只有 1 个垃圾，你却为了这 1 个垃圾，做了 **199,999 次无意义的 `if` 判断**。
* **代价：** 如果查询了 20 万次，你总共执行了约 $4 \times 10^{10}$ 次无效判断。这些判断在 `arr[x][i] == 0` 时只是简单地浪费 CPU 指令，但累积起来就成了天文数字。

原代码把“没垃圾的地方”也当成“待处理的任务”来循环了一遍又一遍。**优化后的代码实现了“按需处理”，只对那 $N$ 个垃圾点进行“点对点”的精准删除，这就是它能通过的原因。