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
        elif i > k:  
            arr[i] = (2 * arr[i-1] - arr[i-k-1] + MOD) % MOD  
        elif i == k:  
            arr[k] = k % MOD  
  
    print(arr[-1])  
  
if __name__ == '__main__':  
    solve()
```