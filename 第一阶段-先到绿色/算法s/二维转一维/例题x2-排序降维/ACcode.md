``` python
import sys
import math

def main():
    
    n = int(sys.stdin.readline().strip())
    
    arr = []
    for _ in range(n):
        x, y = map(int, sys.stdin.readline().strip().split())
        arr.append((x, y))
        
    arr.sort(key=lambda x: x[0])
    
    # print(arr)
    cnt = 0
    minn_y = math.inf
    
    for x, y in arr:
    
        if y < minn_y:
            cnt += 1
            minn_y = y 
        
    print(cnt)
    
if __name__ == '__main__':
    main()
```