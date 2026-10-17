``` python
dist = [ [[-1] * 2  for _ in range(w)] for _ in range(h)]
```

这样初始化很有水平，相当有水平

没有用vistited，

-1 就相当于未访问过
一旦 >= 0 ，就待遇，已访问过且记录了最短距离

