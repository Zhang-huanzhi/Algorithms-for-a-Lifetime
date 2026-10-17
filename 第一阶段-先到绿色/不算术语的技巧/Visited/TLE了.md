``` python
import itertools
import math
import sys
from collections import deque, defaultdict, Counter

sys.setrecursionlimit(10**9)

def main():


    n, m = map(int, sys.stdin.readline().strip().split())

    arr = [set() for _ in range(n+1)]
    for i in range(m):
        x, y = map(int, sys.stdin.readline().strip().split())
        arr[x].add(y)

    # print(arr)

    q = deque([1])

    ans = set()
    while q:
        x = q.popleft()
        if x not in ans:
            ans.add(x)

        for num in arr[x]:
            if num not in ans:
                q.append(num)

    print(len(ans))


if __name__ == '__main__':
    main()

```