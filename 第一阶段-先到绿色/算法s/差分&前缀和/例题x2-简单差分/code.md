``` python
import sys  
  
def solve():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = [0] * (n + 2)  
    diff = [0] * (n + 2)  
  
    for i in range(m):  
        l, r = map(int, sys.stdin.readline().strip().split())  
  
        diff[l] += 1  
        diff[r+1] -= 1  
  
    # 还原arr  
    for i in range(1, n+1):  
        arr[i] += arr[i -1] + diff[i]  
  
    print(min(arr[1:-1]))  
  
if __name__ == '__main__':  
    solve()
```