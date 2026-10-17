### 作用
将字符串中的指定子串替换为新的子串。

### 基本语法
```python
s.replace(old, new, count)
```
- `old`：要被替换的子串
- `new`：替换成的新子串
- `count`（可选）：替换的次数，默认全部替换

### 返回值
返回替换后的新字符串（原字符串不变）

### 示例
```python
s = "apple apple apple"
print(s.replace("apple", "orange"))      # 输出 orange orange orange
print(s.replace("apple", "orange", 2))   # 只替换前2个，输出 orange orange apple
```

### 注意事项
- 不会修改原字符串，而是返回新字符串
- 如果`old`不存在，原字符串不变
- 可以指定替换次数
