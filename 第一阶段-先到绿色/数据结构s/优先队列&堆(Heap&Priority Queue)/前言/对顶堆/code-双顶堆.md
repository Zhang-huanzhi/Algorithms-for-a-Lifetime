``` python
import sys  
import heapq  
  
def solve():  
  
    x = int(sys.stdin.readline().strip())  
    q = int(sys.stdin.readline().strip())  
  
    med = x  
    left_heap = [] # 放比mid小的 ， 最大堆  
    right_heap = [] # 放比mid大的， 最小堆  
  
    out = []  
  
    for i in range(q):  
        a, b = map(int, sys.stdin.readline().strip().split())  
        if a > b: a, b = b, a  
  
  
        if b <= med:  
            heapq.heappush(left_heap, -a)  
            heapq.heappush(left_heap, -b)  
  
            heapq.heappush(right_heap, med)  
            med = -heapq.heappop(left_heap)  
  
        elif a >= med:  
            heapq.heappush(right_heap, a)  
            heapq.heappush(right_heap, b)  
  
            heapq.heappush(left_heap, -med)  
            med = heapq.heappop(right_heap)  
        else:  
            heapq.heappush(left_heap, -a)  
            heapq.heappush(right_heap, b)  
  
        out.append(med)  
  
    print(*out,sep='\n',end='\n')  
  
  
if __name__ == '__main__':  
    solve()
```