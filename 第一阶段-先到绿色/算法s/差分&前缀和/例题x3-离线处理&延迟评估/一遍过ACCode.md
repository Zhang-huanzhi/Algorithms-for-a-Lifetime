``` python
import sys  
import math  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    s = sys.stdin.readline().strip()  
    t = sys.stdin.readline().strip()  
  
    arr = [0] * (n + 1)  
    diff = [0] * (n + 1)  
  
    for i in range(m):  
        l, r = map(int, sys.stdin.readline().strip().split())  
  
        l -= 1  
        r -= 1  
  
        diff[l] += 1  
        diff[r + 1] -= 1  
  
    # 还原arr  
    arr[0] += diff[0]  
    for i in range(1, n):  
        arr[i] = diff[i] + arr[i - 1]  
  
    ns = [''] * n  
  
    for i in range(n):  
        if arr[i] % 2 == 1:  
            ns[i] = t[i]  
        elif arr[i] % 2 == 0:  
            ns[i] = s[i]  
  
    print(''.join(ns))  
  
if __name__ == '__main__':  
    main()
```