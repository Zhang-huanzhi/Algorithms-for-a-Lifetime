``` python
import sys  
import math  
from collections import deque  
  
  
def main():  
  
    h, w = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    for i in range(h):  
        arr.append(list(sys.stdin.readline().strip()))  
  
    dist = [ [[-1] * 2  for _ in range(w)] for _ in range(h)]  
  
    directs = [(1, 0), (-1, 0), (0, 1), (0, -1)]  
  
    q = deque()  
  
    start = (0, 0)  
    end = (0, 0)  
  
    # 起点，终点  
    for i in range(h):  
        for j in range(w):  
            if arr[i][j] == 'S':  
                start = (i, j)  
            elif arr[i][j] == 'G':  
                end = (i , j)  
  
    q.append((start[0], start[1], 0))  
    dist[start[0]][start[1]][0] = 0  
    while q:  
        x, y, curr_state = q.popleft()  
  
        if (x, y) == end:  
            print(dist[x][y][curr_state])  
            return  
  
        for dx, dy in directs:  
            nx = x + dx  
            ny = y + dy  
            # 在范围内  
            if 0 <= nx < h and 0 <= ny < w:  
                cell = arr[nx][ny]  
  
                if cell == '#':  
                    continue  
  
                can_go = False  
                next_state = curr_state  
  
                if cell in ('.', 'S', 'G'):  
                    can_go = True  
                elif cell == '?':  
                    can_go = True  
                    next_state = 1 - curr_state  
                elif cell == 'o':  
                    if curr_state == 0:  
                        can_go = True  
                elif cell == 'x':  
                    if curr_state == 1:  
                        can_go = True  
  
                if can_go and dist[nx][ny][next_state] == -1:  
                    dist[nx][ny][next_state] = dist[x][y][curr_state] + 1  
                    q.append((nx, ny, next_state))  
    print(-1)  
  
if __name__ == '__main__':  
    main()
```