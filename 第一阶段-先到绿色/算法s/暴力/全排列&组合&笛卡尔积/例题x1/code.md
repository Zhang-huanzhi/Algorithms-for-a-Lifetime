``` python
import math  
import sys  
import itertools  
  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    crr = list(map(int, sys.stdin.readline().strip().split()))  
  
    zoo_animals = [[] for _ in range(n)]  
  
    for animal_idx in range(m):  
        row = list(map(int, sys.stdin.readline().strip().split()))  
  
        for zoo_num in row[1:]:  
            zoo_animals[zoo_num - 1].append(animal_idx)  
  
    ans = math.inf  
  
    for combo in itertools.product(range(3), repeat=n):  
  
        brr = [0] * m  
        cost = 0  
  
        for i in range(n):  
            visits = combo[i]  
            if visits > 0:  
                cost += visits * crr[i]  
  
                for animal in zoo_animals[i]:  
                    brr[animal] += visits  
  
        if cost >= ans:  
            continue  
  
        if min(brr) >= 2:  
            ans = cost  
  
    print(ans)  
  
  
if __name__ == '__main__':  
    main()
```