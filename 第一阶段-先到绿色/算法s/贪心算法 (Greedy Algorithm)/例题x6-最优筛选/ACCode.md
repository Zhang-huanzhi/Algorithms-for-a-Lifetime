``` python
import itertools  
import math  
import sys  
from collections import deque, defaultdict  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    for i in range(m):  
        a, b = map(int, sys.stdin.readline().strip().split())  
        arr.append((a, a - b))  
  
    arr.sort(key = lambda x:(x[0], x[1]))  
  
    brr = []  
    for a, c in arr:  
        if not brr:  
            brr.append((a, c))  
        else:  
            if a > brr[-1][0] and c < brr[-1][1]:  
                brr.append((a, c))  
  
    # print(brr)  
  
    ans = 0  
    x = n  
  
    for a, c in reversed(brr):  
        if x >= a:  
            k = (x - a) // c + 1  
            ans += k  
            x -= k * c  
  
    print(ans)  
  
if __name__ == '__main__':  
    main()
```