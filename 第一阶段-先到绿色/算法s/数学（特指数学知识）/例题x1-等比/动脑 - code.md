``` python
import sys  
  
def solve():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
  
        n = int(sys.stdin.readline().strip())  
  
        arr = list(map(int, sys.stdin.readline().strip().split()))  
  
        abs_set = set(abs(x) for x in arr)  
  
        if len(abs_set) == 1:  
            pos_count = sum(1 for x in arr if x > 0)  
            neg_count = sum(1 for x in arr if x < 0)  
  
            if pos_count == n or neg_count == n:  
                print("Yes")  
            elif abs(pos_count - neg_count) <= 1:  
                print("Yes")  
            else:  
                print("No")  
  
        elif len(abs_set) != n:  
            print("No")  
  
        else:  
            arr.sort(key = abs)  
  
            for i in range(2, n):  
                if arr[i] * arr[0] != arr[i-1] * arr[1]:  
                    print("No")  
                    break  
            else:  
                print("Yes")  
  
  
if __name__ == '__main__':  
    solve()
```