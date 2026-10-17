``` python
import math  
import sys  
from collections import deque  
  
def solve():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    state = [0] * (n + 1)  
  
    parent = [0]  
    added = [""]  
  
    state_count = 0  
  
    for _ in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = line[0]  
  
        if op == '1':  
            p = int(line[1])  
            state[p] = state[0]  
  
  
        if op == '2':  
            p = int(line[1])  
            s = line[2]  
  
            state_count += 1  
  
            parent.append(state[p])  
  
            added.append(s)  
  
            state[p] = state_count  
  
        if op == '3':  
            p = int(line[1])  
            state[0] = state[p]  
  
    curr = state[0]  
    ans = []  
  
    while curr != 0:  
        ans.append(added[curr])  
        curr = parent[curr]  
  
    ans.reverse()  
  
    print("".join(ans))  
  
if __name__ == '__main__':  
    solve()
```