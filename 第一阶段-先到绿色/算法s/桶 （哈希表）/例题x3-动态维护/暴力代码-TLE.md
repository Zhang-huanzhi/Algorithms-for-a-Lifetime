``` python

import bisect
import math
import sys
from collections import deque

def solve():

    n, q = map(int, sys.stdin.readline().strip().split())

    arr = [0] * n
    out = []
    for _ in range(q):
        line = list(sys.stdin.readline().strip().split())

        if line[0] == '1':
            x = int(line[1])
            idx = x - 1
            arr[idx] += 1
            if min(arr) == 1:
                for i in range(n):
                    arr[i] -= 1


        if line[0] == '2':
            y = int(line[1])

            cnt = 0
            for num in arr:
                if num >= y:
                    cnt += 1

            out.append(cnt)

    print(*out,sep='\n')
    
if __name__ == '__main__':
    solve()
    
```