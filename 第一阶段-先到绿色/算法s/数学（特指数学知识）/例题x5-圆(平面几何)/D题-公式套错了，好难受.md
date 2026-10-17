https://atcoder.jp/contests/abc467/tasks/abc467_d

---

### D - Concentric Circles 
时间限制: 2 秒 / 内存限制: 1024 MiB

分值 : 425 分

#### 题目描述
在 $xy$ 平面上，是否存在满足以下所有条件的两个圆 $C_1$ 和 $C_2$？（注意，$C_1$ 和 $C_2$ 可能是同一个圆。）

* 不同的两点 $(P_x, P_y)$ 和 $(Q_x, Q_y)$ 在圆 $C_1$ 的圆周上。
* 不同的两点 $(R_x, R_y)$ 和 $(S_x, S_y)$ 在圆 $C_2$ 的圆周上。
* 圆 $C_1$ 和 $C_2$ 的圆心相同（即为同心圆）。

给定 $T$ 个测试用例，请对每个测试用例分别求解。

#### 数据范围与限制
* $1 \le T \le 5 \times 10^4$
* $-10^9 \le P_x, P_y, Q_x, Q_y, R_x, R_y, S_x, S_y \le 10^9$
* $(P_x, P_y) \neq (Q_x, Q_y)$
* $(R_x, R_y) \neq (S_x, S_y)$
* 所有输入的值均为整数。

#### 输入
输入按以下格式从标准输入给出：

$$T$$
$$\text{case}_1$$
$$\text{case}_2$$
$$\vdots$$
$$\text{case}_T$$

每个测试用例 $\text{case}_t$ 的格式如下：

$$P_x \quad P_y \quad Q_x \quad Q_y \quad R_x \quad R_y \quad S_x \quad S_y$$

#### 输出
输出共 $T$ 行。
第 $t$ 行输出第 $t$ 个测试用例的答案。
对于每个测试用例，如果存在满足所有条件的两个圆 $C_1$ 和 $C_2$，则输出 `Yes`；如果不存在，则输出 `No`。

---

#### 样例输入 1
```text
3
2 0 1 1 -1 0 1 2
1 0 -1 0 0 1 0 -1
4 0 3 1 2 0 1 1
```

#### 样例输出 1
```text
Yes
Yes
No
```

#### 样例说明
* **对于第 1 个测试用例**：
  若将圆 $C_1$ 设为圆心在 $(1,0)$、半径为 $1$ 的圆，将圆 $C_2$ 设为圆心在 $(1,0)$、半径为 $2$ 的圆，则满足所有条件（如下图所示）。

* **对于第 2 个测试用例**：
  若将圆 $C_1$ 和 $C_2$ 都设为圆心在 $(0,0)$、半径为 $1$ 的圆，则满足所有条件。
![[Pasted image 20260719150747.png]]

# Code
``` python
import sys  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
        px, py, qx, qy, rx, ry, sx, sy = map(int, sys.stdin.readline().strip().split())  
  
        # 两个点可以确定一条线哟，至少可以确定斜率  
        dx1 = px -qx  
        dy1 = py - qy  
        dx2 = rx - sx  
        dy2 = ry - sy  
  
        c1 = (qx * qx + qy * qy) - (px * px + py * py)  
        c2 = (sx * sx + sy * sy) - (rx * rx + ry * ry)  
        # 还要判断其是否 重合， 重合也可以平行  
        if dx1 * dy2 != dx2 * dy1:  
            print("Yes")  
        else:  
            if dx1 * c2 == dx2 * c1 and dy1 * c2 == dy2 * c1:  
                print("Yes")  
            else:  
                print("No")  
  
if __name__ == "__main__":  
    main()
```