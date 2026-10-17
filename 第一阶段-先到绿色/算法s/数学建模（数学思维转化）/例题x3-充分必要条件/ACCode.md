``` python
import sys  
import math  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    for _ in range(n):  
        a, b, c = map(int, sys.stdin.readline().strip().split())  
  
        # 由最小值决定  
        print(min(a, c, (a + b + c) // 3))  
  
  
  
if __name__ == '__main__':  
    main()
```