``` python
  
import sys  
from collections import Counter  
  
  
def solve():  
  
    n, l = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    brr = [0]  
  
    for i in range(n - 1):  
        brr.append((arr[i] + brr[-1]) % l)  
  
    print(brr)  
    # 是否是等边三角形，判断三条边是否相同，写3个for循环？  
    cnt = 0  
    for i in range(n - 2):  
        for j in range(i + 1, n - 1):  
            for k in range(j + 1, n):  
                # 这样子算的是外弧长  
                # 同意弧长  
                a = abs(brr[i] - brr[j])  
                b = abs(brr[j] - brr[k])  
                c = abs(brr[i] - brr[k])  
                a = min(abs(l - a), a)  
                b = min(abs(l - b), b)  
                c = min(abs(l - c), c)  
  
                if a == b == c != 0:  
                    cnt += 1  
  
    print(cnt)  
  
if __name__ == '__main__':  
    solve()
```