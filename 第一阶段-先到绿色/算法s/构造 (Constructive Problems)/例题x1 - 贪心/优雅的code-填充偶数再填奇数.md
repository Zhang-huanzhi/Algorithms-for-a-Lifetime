
``` python
import bisect  
import heapq  
import math  
import sys  
from collections import deque, defaultdict, Counter  
  
  
def solve():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
        s = sys.stdin.readline().strip()  
        n = len(s)  
  
        counts = Counter(s)  
  
        max_char, max_count = counts.most_common(1)[0]  
  
        if max_count > (n + 1) // 2:  
            print("No")  
            continue  
        else:  
            print("Yes")  
  
        sorted_char = counts.most_common()  
  
        arr = []  
        for char, count in sorted_char:  
            arr.extend([char] * count)  
  
        ans = ['1'] * n  
  
        idx = 0  
        for char in arr:  
            ans[idx] = char  
            idx += 2  
            if idx >= n:  
                idx = 1  
  
        print("".join(ans))  
  
if __name__ == '__main__':  
    solve()
```