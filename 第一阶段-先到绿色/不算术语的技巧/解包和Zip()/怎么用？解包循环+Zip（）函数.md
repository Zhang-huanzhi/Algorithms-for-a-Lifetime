**场景：** 在 `product`、`permutations` 等暴力搜索中，根据决策（0/1）处理复杂数据

**语法模式：**
```python
for 决策变量, (数据变量1, 数据变量2...) in zip(决策列表, 数据列表):
```

### 优雅对比示例

假设 `chances` 是 `(1, 0, 1)`，`arr` 是 `[[10, 5, 2], [20, 8, 5], ...]`

❌ **痛苦的下标法：**

```python
for i in range(n):
    if chances[i] == 1:
        total_cost += arr[i][0]
        total_algo += arr[i][1]
```

✅ **优雅的解包法：**
```python
for is_chosen, (cost, algo, eng) in zip(chances, arr):
    if is_chosen:
        total_cost += cost
        total_algo += algo
        total_eng += eng
```

---

## 总结：编码好习惯手册

| 技巧       | 适用场景                  | 核心好处                             |
| -------- | --------------------- | -------------------------------- |
| 直接解包     | `for x, y in points:` | 告别 `point[0]`, `point[1]`        |
| zip 合并   | `zip(list_a, list_b)` | 告别 `range(len(a))` 和 `list_b[i]` |
| 解包 + zip | 暴力枚举决策与物品属性对应         | 逻辑高度清晰，一眼看出哪个变量加到了哪里             |