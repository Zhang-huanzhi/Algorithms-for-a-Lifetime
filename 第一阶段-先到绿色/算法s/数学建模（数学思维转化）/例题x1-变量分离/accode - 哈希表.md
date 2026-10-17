# 先前写的，这写得什么构思玩意
``` python
import itertools  
import math  
import sys  
from collections import deque, defaultdict, Counter  
from itertools import repeat  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # i + ai = i - ai  
    # 不能用set，会去重，但这题是可以重复的，coutner？  
    counter = Counter()  
    for i in range(n):  
        counter[i + 1 - arr[i]] += 1  
  
    se = set(counter.keys())  
    cnt = 0  
    for i in range(n):  
        if (i + 1 + arr[i]) in se:  
            cnt += counter[i + 1 + arr[i]]  
  
    print(cnt)  
  
if __name__ == '__main__':  
    main()
```

# 优雅-哈希表
``` python
import sys  
from collections import defaultdict  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    xrr = defaultdict(int)  
    ans = 0  
    for i in range(n):  
        x = i + arr[i]  
  
        xrr[x] += 1  
  
        y = i - arr[i]  
        ans += xrr[y]  
  
    print(ans)  
  
  
if __name__ == "__main__":  
    main()
```