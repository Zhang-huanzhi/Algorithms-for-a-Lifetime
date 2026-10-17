`title()` 是 Python 字符串的内置方法，作用是：**将字符串中每个单词的首字母改成大写，其余字母改成小写**。
 
 **O(n) 时间、O(n) 空间**
 
---

| 方法 | 效果 | 场景 |
|------|------|------|
| `title()` | 每个单词首字母大写 | 标题/名称格式 |
| `capitalize()` | 只有第一个字母大写 | 句子开头 |
| `upper()` | 全部大写 | 常数/缩写 |
| `lower()` | 全部小写 | 统一格式 |

```python
s = "hello world"
print(s.title())       # "Hello World"
print(s.capitalize())  # "Hello world"
print(s.upper())       # "HELLO WORLD"
print(s.lower())       # "hello world"
```

