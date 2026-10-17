``` python
# 自己乱作的，模拟，居然对了？wc，我还学个屁动态规划呀 这好像是所谓的贪心？  
import sys  
import bisect  
from collections import Counter, deque  
import math  
import itertools  
  
  
def main():  
    t = int(sys.stdin.readline())  
    for _ in range(t):  
        n = int(sys.stdin.readline())  
        arr = list(map(int, sys.stdin.readline().split()))  
  
        cnt = 0  
        for i in range(1, n):  
            if arr[i] - arr[i-1] > 1:  
                cnt += arr[i] - arr[i-1] - 1  
                arr[i] = arr[i-1] + 1  
  
        arr.reverse()  
        for i in range(1, n):  
            if arr[i] - arr[i-1] > 1:  
                cnt += arr[i] - arr[i-1] - 1  
                arr[i] = arr[i-1] + 1  
  
        print(cnt)  
  
if __name__ == "__main__":  
    main()
```