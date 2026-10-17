``` python
import itertools  
import math  
import sys  
from collections import deque, defaultdict  
from itertools import repeat  
  
def main():  
  
    T = int(sys.stdin.readline().strip())  
  
    for _ in range(T):  
        n, m = map(int, sys.stdin.readline().strip().split())  
        arr = list(map(int, sys.stdin.readline().strip().split()))  
        brr = list(map(int, sys.stdin.readline().strip().split()))  
  
        arr.sort()  
        brr.sort()  
  
        sum_A = sum(arr)  
        sum_B = sum(brr)  
  
        def check(k):  
            for i in range(k):  
                if arr[n - k + i] + brr[n - 1 - i] < m:  
                    return False  
            return True  
        low = 0  
        high = n  
        best_k = 0  
  
        while low <= high:  
            mid = (low + high) // 2  
            if check(mid):  
                best_k = mid  
                low = mid + 1  
            else:  
                high = mid - 1  
  
        ans = sum_A + sum_B - best_k * m  
        print(ans)  
  
if __name__ == '__main__':  
    main()
```