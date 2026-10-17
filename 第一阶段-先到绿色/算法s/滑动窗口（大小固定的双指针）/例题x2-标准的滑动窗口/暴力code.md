``` python
import bisect  
import math  
import sys  
from collections import deque  
  
def solve():  
  
    n, k = map(int, sys.stdin.readline().strip().split())  
  
    arr = [0] * (n + 1)  
    MOD = 10 ** 9  
    for i in range(n+1):  
        if i < k:  
            arr[i] = 1  
        else:  
            for j in range(i-1, i-k-1, -1):  
                arr[i] += arr[j] % 10 ** 9  
  
    print(arr[-1])  
  
if __name__ == '__main__':  
    solve()
```