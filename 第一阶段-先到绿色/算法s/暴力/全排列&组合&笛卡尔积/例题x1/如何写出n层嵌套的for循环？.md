N 是输入给你的，你没法在写代码的时候预测要写多少层嵌套的for循环。

如果N == 4， 你就得写四层循环：
``` python
# 手写 4 层循环
for i in range(3):
    for j in range(3):
        for k in range(3):
            for l in range(3):
                combo = (i, j, k, l)
```