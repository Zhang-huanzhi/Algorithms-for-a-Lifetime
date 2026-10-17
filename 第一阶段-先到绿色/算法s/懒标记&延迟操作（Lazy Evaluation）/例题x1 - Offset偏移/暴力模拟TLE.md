``` python
import sys  
import heapq  
  
from collections import deque  
  
  
def solve():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    xrr = range(1, n + 1)  
    arr = deque(xrr)  
  
    for i in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = line[0]  
  
        if op == '1':  
            p = int(line[1])  
            x = int(line[2])  
            arr[p-1] = x  
  
        if op == '2':  
            p = int(line[1])  
            print(arr[p-1])  
  
        if op == '3':  
            k = int(line[1])  
            for _ in range(k):  
                arr.append(arr.popleft())  
  
  
if __name__ == '__main__':  
    solve()
```