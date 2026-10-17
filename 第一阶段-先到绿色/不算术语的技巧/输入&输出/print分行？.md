# Python print() 函数完全指南：算法竞赛中的输出技巧

## 前言

在算法竞赛中，我们经常面临多种输出要求：空格隔开、换行输出、同行持续输出等。虽然 `sys.stdout.write` 性能最优，但 `print` 的内置参数在处理中等规模数据时提供了极佳的灵活性和可读性。

---

## print() 函数完整签名

```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

---

## 四大核心参数

### 1. sep（分隔符）

**默认值**：`" "`（空格）

**作用**：控制多个输出对象之间的连接符

**常见用法**：

```python
# 换行输出列表
print(*result, sep="\n")

# 紧凑输出（无间隙）
print(a, b, c, sep="")

# 逗号分隔
print(*result, sep=", ")

# 空格分隔（显式指定）
print(*result, sep=" ")
```

---

### 2. end（结尾符）

**默认值**：`"\n"`（换行）

**作用**：决定打印后光标位置

**常见用法**：

```python
# 同行输出（循环中）
for x in arr:
    print(x, end=" ")

# 不换行，等待后续输出
print("Result: ", end="")
print(answer)

# 双换行
print(result, end="\n\n")
```

---

### 3. file（输出流）

**默认值**：`sys.stdout`（标准输出）

**作用**：重定向输出目标

**调试技巧**：

```python
import sys

# 调试信息输出到 stderr（不影响评测）
print("Debug info:", dp, file=sys.stderr)

# 正常答案输出到 stdout
print(answer)
```

> **💡 竞赛核心技巧**：标准错误输出（stderr）通常不被评测系统判分，可以安全地用于调试。

---

### 4. flush（刷新缓冲）

**默认值**：`False`

**作用**：强制立即清空输出缓冲区

**交互题必备**：

```python
# 交互题中确保评测机即时收到输出
print(query, flush=True)
response = input()

# 防止死锁导致 TLE
print(answer, flush=True)
```

---

## 参数速查表

|参数|默认值|作用|竞赛场景|
|---|---|---|---|
|`sep`|`" "`|多参数间连接符|批量输出列表，控制分隔方式|
|`end`|`"\n"`|打印结尾追加符|同行输出，阻止自动换行|
|`file`|`sys.stdout`|输出流对象|使用 `sys.stderr` 调试不影响判题|
|`flush`|`False`|是否立即清空缓冲区|交互题必用，确保实时通信|

---

## 性能对比

对于 2×10⁵ 规模数据：

```python
# 🏆 最快
sys.stdout.write("\n".join(map(str, result)) + "\n")

# 🥈 次快（推荐日常使用）
print(*result, sep="\n")

# 🥉 较慢（避免在大数据中使用）
for x in result:
    print(x)
```

---

## 实战示例

```python
# 示例 1：矩阵输出
matrix = [[1, 2, 3], [4, 5, 6]]
for row in matrix:
    print(*row)

# 示例 2：调试输出
print("Current state:", state, file=sys.stderr)
print(answer)  # 正常输出答案

# 示例 3：交互题
print(f"? {query}", flush=True)
response = int(input())
```

---

## 总结

- **灵活性优先**：使用 `sep` 和 `end` 参数简化代码
- **性能敏感**：大规模数据优先 `sys.stdout.write`
- **调试友好**：善用 `file=sys.stderr`
- **交互题必备**：记得 `flush=True`