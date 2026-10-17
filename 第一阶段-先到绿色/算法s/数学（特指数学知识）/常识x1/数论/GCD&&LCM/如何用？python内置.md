Python 的官方 math 库已经内置了优化后的 GCD 函数。它不仅效率最高，而且能处理多个数字。


---
GCD:
``` python
import math

# 求两个数的 GCD
print(math.gcd(48, 18))  # 输出: 6

# 求多个数的 GCD (3.9+ 新特性)
print(math.gcd(12, 24, 36, 48))  # 输出: 12
```

LCM：
``` python
import math

# 求两个数的 LCM
print(math.lcm(12, 18))  # 输出: 36

# 求多个数的 LCM
print(math.lcm(2, 3, 4)) # 输出: 12
```