``` python
import sys  
  
def solve():  
    n = int(sys.stdin.readline().strip())  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    if n < 4:  
        print(0)  
        return  
  
    brr = []  
    for i in range(1, n):  
        if arr[i] > arr[i-1]:  
            brr.append(1)  
        else:  
            brr.append(-1)  
  
    crr = []  
    current_dir = brr[0]  
    current_len = 1  
  
    for i in range(1, len(brr)):  
        if brr[i] == current_dir:  
            current_len += 1  
        else:  
            crr.append((current_dir, current_len))  
            current_dir = brr[i]  
            current_len = 1  
    crr.append((current_dir, current_len))  
  
    # ---- 下面是补全的逻辑 ----    ans = 0  
    # 遍历 crr，寻找连续的三个块：上升(1) -> 下降(-1) -> 上升(1)  
    for i in range(len(crr) - 2):  
        dir1, len1 = crr[i]  
        dir2, len2 = crr[i + 1]  
        dir3, len3 = crr[i + 2]  
  
        # 如果符合 波浪线型 的斜率变化规律  
        if dir1 == 1 and dir2 == -1 and dir3 == 1:  
            # 组合数 = 左侧上升段的可选后缀数量 * 右侧上升段的可选前缀数量  
            ans += len1 * len3  
  
    print(ans)  
  
if __name__ == '__main__':  
    solve()
```