
### 3. 更推荐的写法（单次遍历法）
在解决此类“成对匹配且有先后顺序”的问题时，更常见、更安全的做法是**边遍历边维护哈希表（One-pass）**。这样可以自然地保证只匹配左侧已出现的元素，而不需要依赖“$A_i \ge 1$”这个特殊性质。

优化后的标准写法如下：

```python
import sys
from collections import Counter

def main():
    # 读取输入
    n = int(sys.stdin.readline().strip())
    arr = list(map(int, sys.stdin.readline().strip().split()))

    counter = Counter()
    cnt = 0

    # 遍历每个位置 j（使用 1-based 索引）
    for j in range(n):
        idx = j + 1
        
        # 1. 寻找左边有多少个满足 i + A_i == j - A_j 的 i
        target = idx - arr[j]
        cnt += counter[target]
        
        # 2. 将当前的 idx + A_j 放入 counter，供后面的元素匹配使用
        counter[idx + arr[j]] += 1

    print(cnt)

if __name__ == '__main__':
    main()
```

### 这种写法的好处：
* **更省空间**：哈希表是动态增加的，不需要一开始就存入所有元素。
* **更具通用性**：即使题目允许 $A_i \le 0$，该写法也绝对不会出错，因为它在结构上就严格保证了只有左边的元素（已加入 `counter` 的元素）才能与当前右边的元素匹配。
