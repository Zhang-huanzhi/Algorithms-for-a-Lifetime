

### 数学公式

两个数的最大公约数（$\gcd$）和最小公倍数（$\text{lcm}$）满足以下性质：
$$A \times B = \gcd(A, B) \times \text{lcm}(A, B)$$

因此，求最小公倍数的正确公式是：
$$\text{lcm}(A, B) = \frac{A \times B}{\gcd(A, B)}$$

> **原理简介**：  
> 设公约数 $g = \gcd(A, B)$，则 $A = g \cdot x$，$B = g \cdot y$（其中 $x, y$ 互质）。  
> 它们的最小公倍数必须包含各自特有的因子，即 $\text{lcm} = g \cdot x \cdot y$。  
> 而 $A \times B = g^2 \cdot x \cdot y$，正好比 $\text{lcm}$ 多乘了一个 $g$，所以除以 $g$ 就是最小公倍数。
