1. **日常小规模计算**：可以用 math.factorial(n)。
    
2. **算法竞赛/取模题目**：**千万别用内置的**。请务必手写循环，并在每一步乘法后加上 % MOD。
    
3. **记住核心原则**：在“余数世界”里，**“先取模再运算”**或**“边运算边取模”**是保证程序不崩溃、不超时的金科玉律。

4. 必要的时候用预处理，先保留很多数阶乘后的结果

``` python
# ❌ 错误/危险的做法 (n 很大时会卡死)
import math
ans = math.factorial(100000) % MOD

# ✅ 正确的算法做法 (耗时极短，内存极省)
def get_factorial_mod(n, mod):
    res = 1
    for i in range(1, n + 1):
        res = (res * i) % mod
    return res
```