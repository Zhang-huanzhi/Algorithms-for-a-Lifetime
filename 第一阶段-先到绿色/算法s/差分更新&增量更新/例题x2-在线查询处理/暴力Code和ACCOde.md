# 优雅
``` python
import sys  
import math  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    ans = 0  
    for i in range(n):  
        ans += min(arr[i], brr[i])  
  
    for i in range(q):  
        c, x, v = sys.stdin.readline().strip().split()  
        x = int(x) - 1  
        v = int(v)  
  
        ans -= min(arr[x], brr[x])  
        if c == 'A':  
            arr[x] = v  
  
        if c == 'B':  
            brr[x] = v  
  
        ans += min(arr[x], brr[x])  
  
        print(ans)  
  
if __name__ == '__main__':  
    main()
```

# 暴力  
``` python
import sys  
import math  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
  
    for i in range(q):  
        ans = 0  
        c, x, v = sys.stdin.readline().strip().split()  
        x = int(x) - 1  
        v = int(v)  
  
        if c == 'A':  
            arr[x] = v  
  
        if c == 'B':  
            brr[x] = v  
  
        for j in range(n):  
            ans += min(arr[j], brr[j])  
  
        print(ans)  
  
if __name__ == '__main__':  
    main()
```