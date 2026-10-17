``` python
import sys
from itertools import combinations

sys.setrecursionlimit(2000)


def solve():

    q = int(sys.stdin.readline().strip())

    arr = []
    pre = [0] * (10**9 + 1)
    offset = 0
    cnt = 0

    for _ in range(q):
        line = list(sys.stdin.readline().strip().split())

        op = line[0]

        if op == '1':
            c = int(line[1])
            x = int(line[2])

            for i in range(c):
                arr.append(x)
                pre[cnt + i + 1] = pre[cnt + i] + arr[cnt + i]

            cnt += c

        if op == '2':
            k = int(line[1])

            print(pre[k + offset] - pre[offset])

            offset += k


if __name__ == '__main__':
    solve()
```


还有另一个：
``` python
import sys
from itertools import combinations

sys.setrecursionlimit(2000)


def solve():

    q = int(sys.stdin.readline().strip())

    arr = []
    offset = 0
    last_ans = 0

    for _ in range(q):
        line = list(sys.stdin.readline().strip().split())

        op = line[0]

        if op == '1':
            c = int(line[1])
            x = int(line[2])

            arr.append((c,x))

        if op == '2':
            k = int(line[1])

            y = k + offset
            ans = 0
            for cnt, val in arr:
                if y > cnt:
                    y -= cnt
                    ans += cnt * val
                else:
                    ans += y * val
                    break

            print(ans-last_ans)
            offset += k
            last_ans = ans



if __name__ == '__main__':
    solve()```