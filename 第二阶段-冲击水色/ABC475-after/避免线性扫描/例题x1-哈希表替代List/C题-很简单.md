https://atcoder.jp/contests/abc481/tasks/abc481_c

---

### C - 银行账户 (Bank Accounts)

**运行时间限制：** 2 秒 / **内存限制：** 1024 MiB  
**分值：** 300 分

#### 题目描述
在 Kowasugi 银行的后院里，幽灵正在种植南瓜。你决定为南瓜们开设银行账户。

在为南瓜开设银行账户时，会为其分配一个名字和一个 ID。在分配 ID 时，会选用当前**尚未被使用过的最小正整数**。

最初，没有开设任何南瓜银行账户。接下来会发生 $Q$ 次以下事件，请按顺序进行处理：

* 一只南瓜走了过来，报上了自己的名字 $s$。
  * 如果已经存在名字为 $s$ 的账户，则输出该账户的 ID。
  * 如果不存在名字为 $s$ 的账户，则以名字 $s$ 新开设一个账户，并输出该账户新分配的 ID。

#### 约束条件
* $Q$ 为整数，且 $1 \le Q \le 2 \times 10^5$
* 在每次事件中，$s$ 均为由小写英文字母组成的字符串，长度满足 $1 \le |s| \le 10$

#### 输入格式
输入从标准输入中按以下格式给出：

$$Q$$
$$\text{query}_1$$
$$\vdots$$
$$\text{query}_Q$$

其中 $\text{query}_i$ 表示第 $i$ 次事件，格式如下：

$$s$$

#### 输出格式
输出 $Q$ 行。  
第 $i$ 行输出在第 $i$ 次事件中应该输出的数值。

---

### 输入样例 1
```plain
8
pumpkin
hobak
pompoen
hobak
pumpkin
boga
boga
hobak
```

### 输出样例 1
```plain
1
2
3
2
1
4
4
2
```

**样例说明 1：**  
事件按如下流程进行：
1. 第 1 次事件中，新开设名字为 `pumpkin`、ID 为 $1$ 的账户。
2. 第 2 次事件中，新开设名字为 `hobak`、ID 为 $2$ 的账户。
3. 第 3 次事件中，新开设名字为 `pompoen`、ID 为 $3$ 的账户。
4. 第 4 次事件中，名字为 `hobak` 的账户已存在，其 ID 为 $2$。
5. 第 5 次事件中，名字为 `pumpkin` 的账户已存在，其 ID 为 $1$。
6. 第 6 次事件中，新开设名字为 `boga`、ID 为 $4$ 的账户。
7. 第 7 次事件中，名字为 `boga` 的账户已存在，其 ID 为 $4$。
8. 第 8 次事件中，名字为 `hobak` 的账户已存在，其 ID 为 $2$。

---

### 输入样例 2
```plain
7
joayo
joayo
joayo
joayo
joayo
joayo
joayo
```

### 输出样例 2
```plain
1
1
1
1
1
1
1
```

# 哈希 AC Code
``` python
import sys  
from collections import defaultdict  
  
def main():  
  
    q = int(sys.stdin.readline().strip())  
  
    arr = defaultdict(int)  
  
    cnt = 1  
  
    outs = []  
    for _ in range(q):  
        s = sys.stdin.readline().strip()  
  
        if s in arr:  
            outs.append(arr[s])  
  
        else:  
            arr[s] = cnt  
            outs.append(arr[s])  
            cnt += 1  
  
    print('\n'.join(map(str, outs)))  
  
if __name__ == "__main__":  
    main()
```