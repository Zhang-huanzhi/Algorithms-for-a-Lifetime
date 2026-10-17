``` python
import math  
import sys  
from collections import deque  
  
def solve():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    adj = [ [] for i in range(n + 1)]  
  
    for i in range(m):  
        x, y, w = map(int, sys.stdin.readline().strip().split())  
  
        adj[x].append((y, w))  
  
    visited = [[False for _ in range(1024) ] for _ in range(n + 1)]  
    # 寻找路径  
    queue = deque([(1, 0)])  
    visited[1][0] = True  
    ans = math.inf  
  
    while queue:  
        u, cur_xor = queue.popleft()  
  
        if u == n:  
            ans = min(ans, cur_xor)  
  
        for v, w in adj[u]:  
            next_xor = cur_xor ^ w  
            if not visited[v][next_xor]:  
                visited[v][next_xor] = True  
                queue.append((v, next_xor))  
  
    if ans == math.inf:  
        print(-1)  
    else:  
        print(ans)  
  
  
if __name__ == '__main__':  
    solve()
```