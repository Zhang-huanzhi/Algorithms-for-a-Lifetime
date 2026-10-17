``` python
  
import sys  
  
def solve():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
  
        n = int(sys.stdin.readline().strip())  
  
        s = sys.stdin.readline().strip()  
  
        num_1 = s.count("1")  
  
        min_sum = 0  
        current_sum = 0  
  
        for char in s:  
            val = 1 if char == '0' else -1  
  
            current_sum += val  
  
            if current_sum  > 0:  
                current_sum = 0  
  
            if current_sum < min_sum:  
                min_sum = current_sum  
  
        ans = num_1 + min_sum  
  
        print(ans)  
  
  
  
if __name__ == '__main__':  
    solve()
```