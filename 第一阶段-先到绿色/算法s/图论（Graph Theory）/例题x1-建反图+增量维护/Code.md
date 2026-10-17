``` python
import sys  
import bisect  
from collections import deque  
  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    # 邻接表  
    arr = [ [] for _ in range(n + 1)]  
    brr = [0] * (n + 1)  
  
    for _ in range(m):  
        x, y = map(int, sys.stdin.readline().strip().split())  
  
        arr[x].append(y)  
  
    q = int(sys.stdin.readline().strip())  
  
    out = []  
    for _ in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = int(line[0])  
        v = int(line[1])  
  
        if op == 1:  
            brr[v] = 1  
  
        if op == 2:  
            # 遍历所有顶点，看看有没有黑色  
            # DFS 找吧  
  
            if brr[v] == 1:  
                out.append("Yes")  
                continue  
  
            q = deque()  
            q.append(v)  
            visited = [False] * (n + 1)  
            visited[v] = True  
            state = False  
            while q:  
                curr = q.pop()  
  
                for next in arr[curr]:  
  
                    if brr[next] == 1:  
                        state = True  
                        break  
                    if not visited[next]:  
                        q.append(next)  
                        visited[next] = True  
  
                if state:  
                    break  
            if state:  
                out.append("Yes")  
            else:  
                out.append("No")  
  
    print('\n'.join(out))  
if __name__ == "__main__":  
    main()
```

暴力代码会超时的主要原因在于**重复计算过多**，导致总计算次数远远超出了运行时间限制。

简要原因如下：

1. **单次询问代价大**：在最坏情况下（例如图中大部分点是白色），每次执行询问 2，DFS 都需要遍历整张图的所有顶点和边，单次复杂度为 $O(N + M)$。
2. **总计算量巨大**：由于有 $Q$ 次询问，整套代码的最坏时间复杂度为 $O(Q \times (N + M))$。
3. **超出计算机处理极限**：
   * 本题数据范围 $N, M, Q \le 3 \times 10^5$。
   * 最坏情况下的计算次数大约为 $3 \times 10^5 \times (3 \times 10^5 + 3 \times 10^5) \approx 1.8 \times 10^{11}$（约 1800 亿次操作）。
   * 3 秒的运行时间限制通常只能支持约 $10^7 \sim 10^8$ 次的 Python 基本操作，因此暴力代码会严重超时。


# 建反图

``` python
import sys  
import bisect  
from collections import deque  
from netrc import netrc  
  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    # 反向图  
    arr = [ [] for _ in range(n + 1)]  
  
    for _ in range(m):  
        x, y = map(int, sys.stdin.readline().strip().split())  
  
        arr[y].append(x)  
  
    q = int(sys.stdin.readline().strip())  
  
    brr = [False] * (n + 1)  
    out = []  
    que = deque()  
    for _ in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = int(line[0])  
        v = int(line[1])  
  
        if op == 1:  
  
            if not brr[v]:  
                brr[v] = True  
                que.append(v)  
  
                while que:  
                    curr = que.popleft()  
                    for nxt in arr[curr]:  
                        if not brr[nxt]:  
                            brr[nxt] = True  
                            que.append(nxt)  
  
        if op == 2:  
            if brr[v]:  
                out.append("Yes")  
            else:  
                out.append("No")  
  
    print('\n'.join(out))  
  
if __name__ == "__main__":  
    main()
```