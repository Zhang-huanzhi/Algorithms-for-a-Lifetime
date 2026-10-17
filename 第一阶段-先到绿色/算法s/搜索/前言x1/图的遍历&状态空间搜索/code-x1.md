``` python
import math  
import sys  
from collections import deque  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    Max_Val = 10 ** 9  
  
    arr = []  
    p = 1  
    while p <= Max_Val:  
        arr.append(p)  
        p *= 2  
  
    arr_str = [str(x) for x in arr]  
  
    brr = set(arr)  
  
    q = deque(arr)  
  
    while q:  
        current = q.popleft()  
        current_str = str(current)  
  
        for s_str in arr_str:  
            new_num = int(current_str + s_str)  
  
            if new_num <= Max_Val and new_num not in arr:  
                brr.add(new_num)  
                q.append(new_num)  
  
    crr = sorted(list(brr))  
  
    print(crr[n - 1])  
  
if __name__ == '__main__':  
    main()1
```