``` python
import sys  
from collections import deque  
  
  
def main():  
    h, w = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    res = []  
    queue = deque()  
  
    dist = [[-1 for _ in range(w)] for _ in range(h)]  
  
    for i in range(h):  
        row = list(sys.stdin.readline().strip())  
        arr.append(row)  
        res.append(row[:])  
        for j in range(w):  
            if row[j] == 'E':  
                queue.append((i, j))  
                dist[i][j] = 0  
  
    directions = [(-1, 0, 'v'), (1, 0, '^'), (0, -1, '>'), (0, 1, '<')]  
  
    while queue:  
        curr_i, curr_j = queue.popleft()  
  
        for dr, dc, arrow in directions:  
            ni, nj = curr_i + dr, curr_j + dc  
  
            if 0 <= ni < h and 0 <= nj < w:  
  
                if arr[ni][nj] == '.' and dist[ni][nj] == -1:  
                    dist[ni][nj] = dist[curr_i][curr_j] + 1  
                    res[ni][nj] = arrow  
                    queue.append((ni, nj))  
  
    for row in res:  
        print("".join(row))  
  
  
if __name__ == '__main__':  
    main()
```