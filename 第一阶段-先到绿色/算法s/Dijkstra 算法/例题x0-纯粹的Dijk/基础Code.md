``` python
import bisect  
import heapq  
import sys  
from collections import deque  
  
  
def main():  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    # 数组大小为 n+1，我们可以直接把 0 号节点作为虚拟中转点  
    arr = [[] for i in range(n + 1)]  
  
    # 读取 m 条双向边  
    for i in range(m):  
        u, v, t = map(int, sys.stdin.readline().strip().split())  
        arr[u].append((v, t))  
        arr[v].append((u, t))  
  
    # 存储到各节点的最短距离，初始化为无穷大  
    cost = [float('inf')] * (n + 1)  
    cost[1] = 0  
  
    # 优先队列中存储 (距离, 节点)  
    q = [(0, 1)]  
  
    while q:  
        d, u = heapq.heappop(q)  
  
        if d > cost[u]:  
            continue  
  
        for v, t in arr[u]:  
            if cost[u] + t < cost[v]:  
                cost[v] = cost[u] + t  
                heapq.heappush(q, (cost[v], v))  
  
    print(*(cost[2:]))  
  
  
if __name__ == '__main__':  
    main()
```