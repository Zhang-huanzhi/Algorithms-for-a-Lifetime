# 算法竞赛中的组合数计算：math.comb()

## 前言

在算法竞赛中，**组合数（从 n 个中选 k 个）** 是高频考点。

很多新手会手写阶乘计算：

```python
# ❌ 容易出错
factorial(n) // (factorial(k) * factorial(n-k))
```

**问题：** 忘记边界判断、性能低、代码冗长。

**Python 3.8+ 提供了 `math.comb(n, k)`，完美解决这些问题。**

---

## 1. 基本功能

计算组合数 $\binom{n}{k} = \frac{n!}{k!(n-k)!}$

```python
from math import comb

print(comb(10, 3))   # 输出 120
print(comb(5, 2))    # 输出 10
```

---

## 2. 三大优势

### ⭐ 自动处理边界（最重要）

|情况|返回值|
|---|---|
|`k > n`|`0`|
|`k = 0` 或 `k = n`|`1`|

```python
# ✅ 无需手动判断
print(comb(2, 5))   # 自动返回 0
print(comb(5, 0))   # 自动返回 1

# ❌ 以前需要写
if n < k:
    print(0)
else:
    print(comb(n, k))
```

### ⚡ 性能极高

- C 语言底层实现，比手写快数十倍
- 智能算法，不会先算巨大阶乘

### 🛡️ 支持超大数

```python
print(comb(100, 50))  # 100891344545564193334812497256
# 无溢出，精确计算
```

---

## 3. 代码对比

### ❌ 手写版本

```python
def my_comb(n, k):
    if k > n: return 0
    if k == 0: return 1
    return factorial(n) // (factorial(k) * factorial(n-k))
```

### ✅ 标准写法

```python
from math import comb
print(comb(n, k))  # 一行搞定
```

---

## 4. 实战示例

### 例题：从 n 个人中选 k 个

```python
from math import comb
n, k = map(int, input().split())
print(comb(n, k))
```

### 例题：杨辉三角

```python
from math import comb
n = 5
for i in range(n):
    print(" ".join(str(comb(i, j)) for j in range(i + 1)))
```

---

## 5. 注意事项

- **参数必须是非负整数**：`comb(5, -1)` 会报错
- **版本要求**：Python 3.8+（现代 OJ 都支持）

---

## 总结口诀

> **看到组合数，首选 math.comb。**  
> **k > n 返回零，边界自动处理。**

### 标准流程

1. `from math import comb`
2. `comb(n, k)` 直接用
3. 无需任何 `if` 判断

**记住：遇到组合数问题，第一反应就是 `math.comb()`！**