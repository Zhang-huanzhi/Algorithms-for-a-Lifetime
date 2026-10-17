``` python
import sys  
import math  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    minn_x = math.inf  
    maxx_x = -math.inf  
    minn_y = math.inf  
    maxx_y = -math.inf  
  
    for i in range(n):  
        x, y = map(int, sys.stdin.readline().strip().split())  
        if x < minn_x:  
            minn_x = x  
        if x > maxx_x:  
            maxx_x = x  
        if y < minn_y:  
            minn_y = y  
        if y > maxx_y:  
            maxx_y = y  
  
    ans = 0  
    ans = max((maxx_x - minn_x + 1) // 2, (maxx_y - minn_y + 1) // 2)  
  
    print(ans)  
  
  
if __name__ == '__main__':  
    main()
```