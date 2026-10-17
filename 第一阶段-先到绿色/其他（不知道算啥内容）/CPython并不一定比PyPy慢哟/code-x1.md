``` python
import math  
import sys  
from collections import deque, defaultdict  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = []  
  
    for i in range(n):  
        x, y = map(int, sys.stdin.readline().strip().split())  
        arr.append((x, y))  
  
    m = int(sys.stdin.readline().strip())  
  
    brr = []  
    for i in range(m):  
        brr.append(sys.stdin.readline().strip())  
  
    crr = set()  
  
    # 还是得建桶  
    for i in range(n):  
        for s in brr:  
            len_s = len(s)  
            for j in range(len(s)):  
                crr.add((len_s, j, s[j]))  
  
  
    for s in brr:  
        if len(s) != n:  
            print("No")  
            continue  
  
        is_ok = [False] * n  
  
        for i in range(n):  
            need_char = s[i]  
            need_len = arr[i][0]  
            need_num = arr[i][1]  
  
  
            if (need_len, need_num-1, need_char) in crr:  
                is_ok[i] = True  
  
            # for s2 in brr:  
            #     if len(s2) == need_len and s2[need_num - 1] == need_char:            #         is_ok[i] = True            #         break  
        for i in range(n):  
            if not is_ok[i]:  
                print("No")  
                break  
        else:  
            print("Yes")  
  
  
if __name__ == '__main__':  
    main()
```