
``` python
import sys  
  
def main():  
  
    n, k, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    for i in range(n):  
        arr.append(tuple(map(int ,sys.stdin.readline().strip().split())))  
  
    arr.sort(key = lambda x:(-x[1], x[0]))  
  
    ans = 0  
    temp = set()  
    cnt = 0  
    visited = [False] * (n + 1)  
  
    for i in range(n):  
        c, value = arr[i][0], arr[i][1]  
  
        if cnt == m:  
            break  
  
        if c not in temp:  
            cnt += 1  
            temp.add(c)  
            visited[i] = True  
            ans += value  
  
    cnt = 0  
    for i in range(n):  
        if cnt == k - m:  
            break  
  
        if not visited[i]:  
            ans += arr[i][1]  
            cnt += 1  
  
    print(ans)  
  
  
if __name__ == '__main__':  
    main()
```