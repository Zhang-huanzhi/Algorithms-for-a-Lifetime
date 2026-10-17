``` python
import math  
import sys  
from collections import deque  
  
def solve():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    brr = [0] * n  
  
    for i in range(q):  
        cnt = 0  
        idx = arr[i] - 1  
  
        if brr[idx] == 0:  
            brr[idx] = 1  
        elif brr[idx] == 1:  
            brr[idx] = 0  
  
        if brr[0] == 1:  
            cnt += 1  
  
        for j in range(1, n):  
            if  brr[j] == 1 and brr[j-1] == 0:  
                cnt +=1  
  
        print(cnt)  
  
if __name__ == '__main__':  
    solve()
```