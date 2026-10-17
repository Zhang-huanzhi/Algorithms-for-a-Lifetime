当前算法的时间复杂度是 $O(Q \times N)$，在最坏情况下计算量达到了 $5 \times 10^5 \times 10000 = 5 \times 10^9$ 次操作，这在 2 秒内是无法通过的。

``` python
import itertools  
import math  
import sys  
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
  
    Q = int(sys.stdin.readline().strip())  
  
    for _ in range(Q):  
        x = int(sys.stdin.readline().strip())  
  
        for p, a, b in zip(prr, arr, brr):  
  
            if p >= x:  
                x += a  
            elif p < x:  
                if x >= b:  
                    x -= b  
                else:  
                    x = 0  
  
        print(x)  
  
  
  
if __name__ == '__main__':  
    main()```