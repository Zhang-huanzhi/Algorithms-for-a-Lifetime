# 易读
``` python
import sys

input = sys.stdin.readline

n = int(input())

total_weight = 0
answer = 0
items = []

for _ in range(n):
    w, h, b = map(int, input().split())

    total_weight += w
    answer += b

    # 把配件放到头部后，喜悦值增加多少
    value = h - b

    if value > 0:
        items.append((w, value))

# 头部最多能放的重量
limit = total_weight // 2

# dp[x] 表示头部重量不超过 x 时，最多增加的喜悦值
dp = [0] * (limit + 1)

for w, value in items:
    # 必须从大到小枚举，保证每个配件只使用一次
    for weight in range(limit, w - 1, -1):
        dp[weight] = max(
            dp[weight],
            dp[weight - w] + value
        )

print(answer + dp[limit])
```
# 高级
``` python
import sys


def main():
    input = sys.stdin.buffer.readline

    N = int(input())

    total_weight = 0
    base_value = 0
    items = []

    for _ in range(N):
        W, H, B = map(int, input().split())

        total_weight += W
        base_value += B

        gain = H - B
        if gain > 0:
            items.append((W, gain))

    capacity = total_weight // 2

    # dp[x]：头部重量不超过 x 时，最多增加的喜悦值
    dp = [0] * (capacity + 1)

    # 当前处理过的物品总重量上限，减少无效计算
    reachable = 0

    for weight, gain in items:
        new_reachable = min(capacity, reachable + weight)

        # 逆序 0/1 背包，使用切片和列表推导提高 Python 执行效率
        dp[weight:new_reachable + 1] = [
            max(current, previous + gain)
            for current, previous in zip(
                dp[weight:new_reachable + 1],
                dp[:new_reachable + 1 - weight]
            )
        ]

        reachable = new_reachable

    print(base_value + dp[capacity])


if __name__ == "__main__":
    main()
```