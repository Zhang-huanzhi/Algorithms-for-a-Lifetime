
 ***y // 2 - (x - 1) // 2***

---


**思路：**
- 区间 $[x, y]$ 内的偶数个数 = $[1, y]$ 内偶数个数 $-$ $[1, x-1]$ 内偶数个数
- $[1, n]$ 内偶数个数就是 $\left\lfloor \frac{n}{2} \right\rfloor$

**公式：**
$$
\text{偶数个数} = \left\lfloor \frac{y}{2} \right\rfloor - \left\lfloor \frac{x-1}{2} \right\rfloor
$$

**代码示例：**
```python
def count_even(x, y):
    return y // 2 - (x - 1) // 2

# 例：统计 3~10 之间有多少偶数
print(count_even(3, 10))  # 输出 4（4, 6, 8, 10）
```

**技巧说明：**
- 直接用整除运算，避免循环遍历，时间复杂度 $O(1)$，非常高效。
- 这个技巧常用于区间统计奇偶数、倍数等问题。