``` python

import sys
from collections import Counter

def solve():

    x = int(sys.stdin.readline().strip())
    q = int(sys.stdin.readline().strip())

    arr = [x]

    for i in range(q):
        a, b = map(int, sys.stdin.readline().strip().split())
        arr.append(a)
        arr.append(b)

        arr.sort()
        print(arr[len(arr)//2])


if __name__ == '__main__':
    solve()
```