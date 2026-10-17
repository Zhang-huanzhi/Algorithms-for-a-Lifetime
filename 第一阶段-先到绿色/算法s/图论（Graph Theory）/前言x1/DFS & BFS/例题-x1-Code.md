# DFS
```  python
import sys  
import bisect  
from collections import Counter, deque  
import math  
import itertools  
  
import numpy as np  
from fontTools.ttLib.ttVisitor import visit  
  
sys.setrecursionlimit(1000000)  
  
def main():  
  
    r, c = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
    for _ in range(r):  
        line = sys.stdin.readline().strip()  
        arr.append(list(line))  
  
    # 找起点  
    start_x, start_y = -1, -1  
    for i in range(r):  
        for j in range(c):  
            if arr[i][j] == 'S':  
                start_x, start_y = i, j  
                break  
        if start_x != -1:  
            break  
  
    # DFS  
    visited = [[False] * c for _ in range(r)]  
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]  
  
    def dfs(x, y):  
        if arr[x][y] == 'E':  
            return True  
  
        visited[x][y] = True  
  
        for dx, dy in directions:  
            nx, ny = x + dx, y + dy  
  
            if 0 <= nx < r and 0 <= ny < c and not visited[nx][ny] and arr[nx][ny] != '#':  
                if dfs(nx, ny):  
                    return True  
  
        return False  
  
  
    if dfs(start_x, start_y):  
        print('Yes')  
    else:  
        print('No')  
  
  
if __name__ == "__main__":  
    main()
```


# BFS
``` python

import sys
import bisect
from collections import Counter, deque
import math
import itertools

import numpy as np


def main():
    r, c = map(int, sys.stdin.readline().split())

    arr = []
    for _ in range(r):
        line = sys.stdin.readline().split()
        arr.append(list(line))

    # 找起点
    start_x, start_y = -1, -1
    for i in range(r):
        for j in range(c):
            if arr[i][j] == 'S':
                start_x, start_y = i, j
                break
        if start_x != -1:
            break

    # BFS扩散
    visited = [[False] * c for _ in range(r)]
    is_founded = False

    q = deque([(start_x, start_y)])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    while q:
        x, y = q.popleft()
        if arr[x][y] == 'E':
            is_founded = True
            break

        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            # 在墙内
            if 0 <= nx < r and 0 <= ny < c:
                # 不是阻拦且没有被访问过
                if arr[nx][ny] != '#' and not visited[nx][ny]:
                    visited[nx][ny] = True
                    q.append((nx, ny))

    if is_founded:
        print('YES')
    else:
        print('NO')

if __name__ == "__main__":
    main()

```