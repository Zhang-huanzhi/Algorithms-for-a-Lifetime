# 显性数组 看得懂点
``` python
import sys

def main():
    # 读取输入
    n = int(sys.stdin.readline().strip())
    t = sys.stdin.readline().strip()

    # 第一步：把字符串映射成由 1 和 -1 组成的数值数组 A
    A = []
    for char in t:
        if char == '1':
            A.append(1)
        else:
            A.append(-1)

    # 第二步：构建前缀积数组 P
    # P 的长度为 n + 1，因为要包含一个初始值 P[0] = 1
    P = [0] * (n + 1)
    P[0] = 1  # 规定起始前缀积为 1

    # 依次计算 P[1], P[2], ..., P[n]
    for i in range(n):
        P[i + 1] = P[i] * A[i]

    # 第三步：统计前缀积数组 P 中 1 和 -1 各自出现的次数
    count_1 = P.count(1)
    count_minus_1 = P.count(-1)

    # 第四步：使用组合数公式计算结果
    ans = (count_1 * (count_1 - 1)) // 2 + (count_minus_1 * (count_minus_1 - 1)) // 2

    print(ans)

if __name__ == '__main__':
    main()
```
# 临时变量省空间版本
``` python
import math  
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    t = sys.stdin.readline().strip()  
  
    count_1 = 1  
    cnt = 0  
  
    current_product = 1  
  
    ans = 0  
    for char in t:  
        if char == '0':  
            current_product *= -1  
  
        if current_product == 1:  
            count_1 += 1  
        else:  
            cnt += 1  
  
        ans = (count_1 * (count_1 - 1)) // 2 + (cnt * (cnt - 1)) // 2  
  
    print(ans)  
  
  
  
if __name__ == '__main__':  
    main()
```