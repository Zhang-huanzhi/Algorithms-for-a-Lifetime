``` python
import math  
import sys  
from collections import deque  
  
def solve():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    brr = [0] * (n + 2)  
  
    cnt = 0  
    for i in range(q):  
  
        idx = arr[i]  
  
        if brr[idx] == 0:  
            brr[idx] = 1  
  
            if brr[idx - 1] == 0 and brr[idx + 1] == 0:  
                cnt += 1  
  
            if brr[idx - 1] == 1 and brr[idx + 1] == 1:  
                cnt -= 1  
  
        elif brr[idx] == 1:  
            brr[idx] = 0  
  
            if brr[idx - 1] == 1 and brr[idx + 1] == 1:  
                cnt += 1  
  
            if brr[idx - 1] == 0 and brr[idx + 1] == 0:  
                cnt -= 1  
  
        print(cnt)  
  
if __name__ == '__main__':  
    solve()
```