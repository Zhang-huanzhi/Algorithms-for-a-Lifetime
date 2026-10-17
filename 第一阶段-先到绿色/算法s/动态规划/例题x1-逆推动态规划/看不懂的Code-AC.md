```python
import itertools  
import math  
import sys  
from bisect import bisect_left  
from collections import deque, defaultdict, Counter  
from itertools import repeat  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    prr = []  
    arr = []  
    brr = []  
    for _ in range(n):  
        p, a, b = map(int, sys.stdin.readline().strip().split())  
        prr.append(p)  
        arr.append(a)  
        brr.append(b)  
  
    pref = [0] * (n + 1)  
    for i in range(n):  
        pref[i + 1] = pref[i] + brr[i]  
  
    dp = [[0] * 1001 for _ in range(n + 1)]  
    for x in range(1001):  
        dp[n][x] = x  
  
    for i in range(n -1, -1, -1):  
        p, a, b = prr[i], arr[i], brr[i]  
        dpi = dp[i]  
        dpi1 = dp[i + 1]  
        for x in range(1001):  
            if x <= p:  
                dpi[x] = dpi1[x + a]  
            else:  
                nxt = x - b  
                if nxt < 0:  
                    nxt = 0  
                dpi[x] = dpi1[nxt]  
  
    Q = int(sys.stdin.readline().strip())  
  
    out = []  
    for _ in range(Q):  
        x = int(sys.stdin.readline().strip())  
  
        if x <= 1000:  
            out.append(str(dp[0][x]))  
        else:  
            d = x - 1000  
            k = bisect_left(pref, d)  
            if k <= n:  
                rem = x - pref[k]  
                out.append(str(dp[k][rem]))  
            else:  
                out.append(str(max(0, x - pref[n])))  
  
    print('\n'.join(out))  
  
if __name__ == '__main__':  
    main()
```