``` python
import itertools
import math
import sys
from collections import deque, defaultdict



def main():

    n, m = map(int, sys.stdin.readline().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    brr = list(map(int, sys.stdin.readline().strip().split()))

    arr.sort()
    brr.sort()

    idxA = 0
    idxB = 0

    cnt = 0
    while True:
        if idxA == n :
            print(cnt)
            break
        elif idxB == m :
            print(cnt)
            break

        if arr[idxA] * 2 >= brr[idxB]:
            idxA += 1
            idxB += 1
            cnt += 1
        else:
            idxA += 1

if __name__ == '__main__':
    main()
```