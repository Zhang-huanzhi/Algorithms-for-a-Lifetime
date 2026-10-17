Python 已经内置了快速幂，**不需要手写**！

---

```python
# 基础用法
pow(a, n)           # 计算 a^n

# 取模用法（三参数版本）
pow(a, n, mod)      # 计算 (a^n) % mod，自动优化
```

**示例：**
```python
# 计算 3^13
print(pow(3, 13))                    # 1594323

# 计算 2^1000 mod (10^9+7)
MOD = 10**9 + 7
print(pow(2, 1000, MOD))             # 140625001

# 计算逆元（费马小定理）
a = 5
p = 1000000007
inv_a = pow(a, p-2, p)               # a的逆元
```

### **手写模板（面试/理解原理用）**

```python
def quick_pow(a, n):
    """基础快速幂"""
    res = 1
    while n:
        if n & 1:      # n % 2 == 1
            res *= a
        a *= a
        n >>= 1        # n //= 2
    return res

def quick_pow_mod(a, n, mod):
    """带取模快速幂"""
    res = 1
    a %= mod
    while n:
        if n & 1:
            res = res * a % mod
        a = a * a % mod
        n >>= 1
    return res
```

