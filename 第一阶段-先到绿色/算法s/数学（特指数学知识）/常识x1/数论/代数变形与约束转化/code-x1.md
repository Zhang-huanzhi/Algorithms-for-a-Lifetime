``` python
import math  
import sys  
  
  
def main():  
    n, x, y = map(int, sys.stdin.readline().split())  
    arr = list(map(int, sys.stdin.readline().split()))  
  
    d = y - x  
  
    # 公共重量 W 必须满足：  
    # max(Ai * x) <= W <= min(Ai * y)  
    l = max(arr) * x  
    r = min(arr) * y  
    if l > r:  
        print(-1)  
        return  
  
    # 对每个孩子：  
    # W = Ai * x + bi * (y - x)  
    # 所以 W ≡ Ai * x (mod d)    # 要对所有 i 都可行，则 Ai * x mod d 必须相同  
    g = math.gcd(x, d)  
    m = d // g  
  
    base = arr[0] % m  
    for a in arr:  
        if a % m != base:  
            print(-1)  
            return  
  
    # 找区间 [l, r] 内最大的满足 W ≡ arr[0] * x (mod d) 的 W    residue = (arr[0] * x) % d  
    w = r - ((r - residue) % d)  
  
    if w < l:  
        print(-1)  
        return  
  
    # 总大糖数 = sum((W - Ai*x)/d)    ans = (n * w - x * sum(arr)) // d  
    print(ans)  
  
  
if __name__ == '__main__':  
    main()
```