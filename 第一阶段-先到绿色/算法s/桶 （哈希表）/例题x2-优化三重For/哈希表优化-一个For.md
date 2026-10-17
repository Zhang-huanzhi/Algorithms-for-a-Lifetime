``` python
  
import sys  
from collections import Counter  
  
  
def solve():  
  
    n, l = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    if l % 3 != 0:  
        print(0)  
        return  
  
    s = l // 3  
  
    brr = Counter()  
    curr = 0  
    brr[0] += 1  
  
    for i in range(n - 1):  
        curr = (curr + arr[i]) % l  
        brr[curr] += 1  
  
    cnt = 0  
  
    for p in brr:  
  
        if p < s:  
  
            pos1 = p + s  
            pos2 = p + s + s  
  
            if pos1 in brr and pos2 in brr:  
                cnt += brr[p] * brr[pos1] * brr[pos2]  
  
    print(cnt)  
  
if __name__ == '__main__':  
    solve()
```