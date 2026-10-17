``` python
import bisect  
import math  
import sys  
from collections import deque  
  
def solve():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
        n = int(sys.stdin.readline().strip())  
        arr = list(map(int, sys.stdin.readline().strip().split()))  
  
        cnt = 1  
        curr = arr[0]  
        target = arr[-1]  
        arr.sort()  
        possible = True  
  
        if target <= curr * 2:  
            print(2)  
            continue  
  
        while True:  
  
            if target <= curr * 2:  
  
                cnt += 1  
                # print(cnt)  
                break  
  
            idx = bisect.bisect_right(arr, curr * 2)  
  
            if idx == 0:  
                possible = False  
                break  
            next_curr = arr[idx - 1]  
  
            if next_curr <= curr:  
                possible = False  
                break  
            cnt += 1  
            curr = next_curr  
  
        if possible:  
            print(cnt)  
        else:  
            print(-1)  
  
  
if __name__ == '__main__':  
    solve()
```