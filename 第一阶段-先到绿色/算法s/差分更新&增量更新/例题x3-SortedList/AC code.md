# SortedList 方针2
``` python
import sys  
from sortedcontainers import SortedList  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    crr = SortedList([0])  
    res = []  
    ans = 0  
    # 原来值为多少？  
    brr = {}  
  
    for num in arr:  
  
        crr.add(num)  
        idx = crr.index(num)  
  
        left = crr[idx - 1]  
        right = crr[idx + 1] if idx + 1 < len(crr) else None  
  
        if len(crr) == 2:  
            brr[left] = num - left  
            brr[num] = num - left  
            ans = brr[left] + brr[num]  
            res.append(ans)  
            continue  
  
        old_left = brr[left]  
        brr[left] = min(old_left, num - left)  
        ans += brr[left] - old_left  
  
        if right is not None:  
            old_right = brr[right]  
            brr[right] = min(old_right, right - num)  
            ans += brr[right] - old_right  
  
        if right is None:  
            brr[num] = num - left  
        else:  
            brr[num] = min(num - left, right - num)  
  
        ans += brr[num]  
        res.append(ans)  
  
    print('\n'.join(map(str, res)))  
  
  
if __name__ == "__main__":  
    main()

```


# 方针1

``` python
import sys
from sortedcontainers import SortedList

def main():
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    # 元素为 (坐标, 人编号)，按坐标有序
    crr = SortedList([(0, 0)])

    # brr[i]：人 i 当前的最近距离
    # brr[0] 初始为 0，只是占位；第一个人到达后会被正确更新
    brr = [0] * (n + 1)

    ans = 0
    res = []

    def get_distance(pos):
        """计算 crr[pos] 这个人的最近距离。"""
        x, _ = crr[pos]

        if pos == 0:
            return crr[1][0] - x

        if pos == len(crr) - 1:
            return x - crr[pos - 1][0]

        return min(
            x - crr[pos - 1][0],
            crr[pos + 1][0] - x
        )

    for i, num in enumerate(arr, start=1):
        crr.add((num, i))
        idx = crr.index((num, i))

        # 因 Xi >= 1，0 一定在新人的左边
        left_pos = idx - 1
        left_id = crr[left_pos][1]

        # 插入后右邻居存在时，它的下标是 idx + 1
        right_pos = idx + 1 if idx + 1 < len(crr) else None

        # 1. 先减去受影响的旧贡献
        ans -= brr[left_id]

        if right_pos is not None:
            right_id = crr[right_pos][1]
            ans -= brr[right_id]

        # 2. 插入后，重新计算左邻居和新人
        brr[left_id] = get_distance(left_pos)
        brr[i] = get_distance(idx)

        ans += brr[left_id]
        ans += brr[i]

        # 3. 右邻居存在时，重新计算右邻居
        if right_pos is not None:
            brr[right_id] = get_distance(right_pos)
            ans += brr[right_id]

        res.append(ans)

    print("\n".join(map(str, res)))

if __name__ == "__main__":
    main()
```