``` python
import sys  
import heapq  
  
from collections import deque  
  
  
def solve():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(range(1, n + 1))  
  
    offset = 0  
  
    for i in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = line[0]  
  
        if op == '1':  
            p = int(line[1])  
            x = int(line[2])  
            actual_idx = (offset + p - 1) % n  
            arr[actual_idx] = x  
  
        if op == '2':  
            p = int(line[1])  
            actual_idx = (offset + p - 1) % n  
            print(arr[actual_idx])  
  
  
        if op == '3':  
            k = int(line[1])  
            offset = (offset + k) % n  
  
  
if __name__ == '__main__':  
    solve()
```