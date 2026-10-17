``` python
import sys  
from collections import deque  
  
  
def main():  
    # 使用更快的IO  
    h, w = map(int, sys.stdin.readline().split())  
    grid_data = sys.stdin.read().splitlines()  
    grid = [list(line) for line in grid_data]  
  
    start = None  
    goal = None  
    for i in range(h):  
        for j in range(w):  
            if grid[i][j] == "S":  
                start = (i, j)  
            if grid[i][j] == 'G':  
                goal = (i, j)  
  
    # 方向向量: 0:上, 1:下, 2:左, 3:右  
    directions = [(-1, 0, 'U'), (1, 0, 'D'), (0, -1, 'L'), (0, 1, 'R')]  
  
    # --- 优化点: 状态压缩 ---    # 将三维状态 (r, c, dir) 映射到一维索引  
    # index = r * w * 4 + c * 4 + dir  
    total_states = h * w * 4  
  
    # 使用一维列表代替三维列表  
    visited = [False] * total_states  
    parent = [-1] * total_states  # parent[state_idx] = prev_state_idx  
  
    q = deque()  
  
    # 初始化  
    sr, sc = start  
    for i in range(4):  
        dr, dc, _ = directions[i]  
        nr, nc = sr + dr, sc + dc  
  
        if 0 <= nr < h and 0 <= nc < w and grid[nr][nc] != '#':  
            # 计算当前状态的索引  
            state_idx = nr * w * 4 + nc * 4 + i  
            if not visited[state_idx]:  
                visited[state_idx] = True  
                # 起点的父状态用特殊值 -2 表示 (因为-1是parent的默认值)  
                # 这里我们直接记录父坐标和方向，回溯时再计算索引  
                parent[state_idx] = (sr, sc, -1)  
                q.append((nr, nc, i))  
  
    end_state_tuple = None  
  
    while q:  
        r, c, from_dir_idx = q.popleft()  
  
        if (r, c) == goal:  
            end_state_tuple = (r, c, from_dir_idx)  
            break  
  
        current_cell = grid[r][c]  
  
        for next_dir_idx in range(4):  
            if current_cell == 'o' and next_dir_idx != from_dir_idx:  
                continue  
            if current_cell == 'x' and next_dir_idx == from_dir_idx:  
                continue  
  
            dr, dc, _ = directions[next_dir_idx]  
            nr, nc = r + dr, c + dc  
  
            if 0 <= nr < h and 0 <= nc < w and grid[nr][nc] != '#':  
                next_state_idx = nr * w * 4 + nc * 4 + next_dir_idx  
                if not visited[next_state_idx]:  
                    visited[next_state_idx] = True  
                    # parent现在记录的是上一个状态的(r, c, dir)元组，而不是索引  
                    # 这样回溯时更方便，避免了从索引反推坐标  
                    parent[next_state_idx] = (r, c, from_dir_idx)  
                    q.append((nr, nc, next_dir_idx))  
  
    if end_state_tuple is None:  
        print("No")  
    else:  
        print("Yes")  
        path = []  
        r, c, dir_idx = end_state_tuple  
  
        while dir_idx != -1:  
            path.append(directions[dir_idx][2])  
            # 从parent数组中获取上一个状态的元组  
            state_idx = r * w * 4 + c * 4 + dir_idx  
            pr, pc, p_dir_idx = parent[state_idx]  
            r, c, dir_idx = pr, pc, p_dir_idx  
  
        print("".join(reversed(path)))  
  
  
if __name__ == "__main__":  
    main()
```