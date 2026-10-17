``` python
import math  
import sys  
from collections import deque  
  
def solve():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = [""] * (n + 1)  
  
    for _ in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = line[0]  
  
        if op == '1':  
            p = int(line[1])  
            arr[p] = arr[0]  
  
        if op == '2':  
            p = int(line[1])  
            s = line[2]  
            arr[p] += s  
  
  
        if op == '3':  
            p = int(line[1])  
            arr[0] = arr[p]  
  
    # print(arr)  
    print(arr[0])  
  
if __name__ == '__main__':  
    solve()
```