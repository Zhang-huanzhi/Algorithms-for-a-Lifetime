``` python
import sys  
  
def main():  
  
    s = sys.stdin.readline().strip()  
    MOD = 998244353  
  
    n = len(s)  
  
    cnt = 0  
    ans = 0  
  
    for i in range(0, n):  
  
        if i > 0 and s[i] == s[i - 1]:  
            cnt = 1  
        else:  
            cnt += 1  
  
        ans = (ans + cnt) % MOD  
  
    print(ans)  
  
  
if __name__ == '__main__':  
    main()
```