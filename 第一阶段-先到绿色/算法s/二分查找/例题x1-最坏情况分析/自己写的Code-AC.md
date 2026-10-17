``` python
import itertools  
import math  
import sys  
from bisect import bisect_left, bisect_right  
from collections import deque, defaultdict, Counter  
  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    arr.sort()  
  
    pre = [0] * (n + 1)  
    for i in range(1, n+1):  
        pre[i] = pre[i-1] + arr[i-1]  
  
    # print(pre)  
    # 暴力模拟下  
    for _ in range(q):  
        x = int(sys.stdin.readline().strip())  
  
        ans = 0  
        # 找到第一个稍微大于x的坐标，没有则返回len(arr）  
        idx = bisect_left(arr, x)  
  
        # print(idx)  
  
        ans += pre[idx]  
  
        rem = n - idx  
  
        ans += rem * (x - 1) + 1  
  
        if ans > pre[n]:  
            print("-1")  
        else:  
            print(ans)  
  
if __name__ == '__main__':  
    main()
```