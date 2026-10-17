``` python
import sys  
from collections import defaultdict  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
  
    cnt = [defaultdict(int) for _ in range(11)]  
  
    for x in arr:  
        d = len(str(x))  
        cnt[d][x % m] += 1  
  
    pow10 = [1] * 11  
    for d in range(1, 11):  
        pow10[d] = (pow10[d - 1] * 10) % m  
  
    ans = 0  
  
    for x in arr:  
        xm = x % m  
        for d in range(1, 11):  
            need = (-xm * pow10[d]) % m  
            ans += cnt[d][need]  
  
    print(ans)  
  
if __name__ == '__main__':  
    main()
```