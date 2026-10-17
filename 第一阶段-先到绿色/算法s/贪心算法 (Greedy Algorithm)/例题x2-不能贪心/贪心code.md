``` python
import itertools  
import math  
import sys  
from collections import deque, defaultdict  
  
  
sys.setrecursionlimit(10**9)  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    curr = 0.5  
  
    cnt = 0  
    for i in range(n):  
        if curr > 0:  
            curr -= arr[i]  
            if curr < 0:  
                cnt += 1  
        elif curr < 0:  
            curr += arr[i]  
            if curr > 0:  
                cnt += 1  
  
    print(cnt)  
  
if __name__ == '__main__':  
    main()
```