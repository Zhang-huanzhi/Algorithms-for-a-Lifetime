``` python
import itertools  
import math  
import sys  
from collections import deque, defaultdict  
from itertools import repeat  
  
  
def main():  
  
    n, k, x = map(int, sys.stdin.readline().strip().split())  
  
    # 暴力  
    arr = []  
  
    for _ in range(n):  
        arr.append(sys.stdin.readline().strip())  
  
    # 生成所有  
    brr = []  
    for xrr in itertools.product(range(n), repeat = k):  
        s = ''  
        for i in xrr:  
            s += arr[i]  
  
        brr.append(s)  
  
    brr.sort()  
  
    print(brr[x - 1])  
  
  
if __name__ == '__main__':  
    main()
```