https://atcoder.jp/contests/abc419/tasks/abc419_d

---

### D - Substr Swap

时间限制: 2 sec / 空间限制: 1024 MiB

分值：400 分

#### 题目描述

给定长度为 $N$ 的由小写英文字母组成的字符串 $S$ 和 $T$，以及 $M$ 组整数对 $(L_1, R_1), (L_2, R_2), \dots, (L_M, R_M)$。

按 $i = 1, 2, \dots, M$ 的顺序依次进行以下操作：

* 将 $S$ 的第 $L_i$ 个字符到第 $R_i$ 个字符，与 $T$ 的第 $L_i$ 个字符到第 $R_i$ 个字符进行交换。

例如，当 $S$ 为 `abcdef`，$T$ 为 `ghijkl`，且 $(L_i, R_i) = (3, 5)$ 时，操作后的 $S$ 和 $T$ 分别变为 `abijkf` 和 `ghcdel`。

请输出进行 $M$ 次操作后的字符串 $S$。

#### 数据范围与约束

* $1 \le N \le 5 \times 10^5$
* $1 \le M \le 2 \times 10^5$
* $S$ 和 $T$ 是长度为 $N$ 的由小写英文字母组成的字符串。
* $1 \le L_i \le R_i \le N$
* $N, M, L_i, R_i$ 均为整数。

#### 输入格式

输入通过标准输入以下列格式给出：

$$
N \quad M \\
S \\
T \\
L_1 \quad R_1 \\
L_2 \quad R_2 \\
\vdots \\
L_M \quad R_M
$$

#### 输出格式

输出进行 $M$ 次操作后的字符串 $S$。

---

#### 输入样例 1

```text
5 3
apple
lemon
2 4
1 5
5 5
```

#### 输出样例 1

```text
lpple
```

**样例说明 1：**

初始时，$S$ 和 $T$ 分别为 `apple` 和 `lemon`。
* $i=1$ 操作后：$S, T$ 分别变为 `aemoe` 和 `lppln`。
* $i=2$ 操作后：$S, T$ 分别变为 `lppln` 和 `aemoe`。
* $i=3$ 操作后：$S, T$ 分别变为 `lpple` 和 `aemon`。

因此，进行 $3$ 次操作后的字符串 $S$ 为 `lpple`。

#### 输入样例 2

```text
10 5
lemwrbogje
omsjbfggme
5 8
4 8
1 3
6 6
1 4
```

#### 输出样例 2

```text
lemwrfogje
```