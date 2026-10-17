# 小结
时间和空间上，其实没区别，都是o(n)

但实际跑起来，DFS会快一点，可能是因为直接用的List去模拟栈Stack吧，也不需要额外的操作了，List.pop()常数开销更低。

![[Pasted image 20260623150423.png]]

这两个写起来都没啥区别，感觉唯一的变化，

是出队/栈的时候：

一个用的是 popleft， 一个用到是pop

虽然说这道题上，两者确实没有任何区别，先进先出和先进后厨两者在这道题上确实没区别。

但还是稍微做点笔记吧，对比一下，理解下什么是广度优先，深度优先，呃呃呃，其实应该先说共同点的，这两者都是 **搜索算法**，而且都是暴力搜索。


| 维度         | BFS 解法 (广度优先)                                        | DFS 解法 (深度优先)                                  |
| ---------- | ---------------------------------------------------- | ---------------------------------------------- |
| **核心数据结构** | **队列 (Queue)**：通过 deque.popleft() 实现 **FIFO**（先进先出）。 | **栈 (Stack)**：通过 list.pop() 实现 **LIFO**（后进先出）。 |
| **遍历顺序特征** | **层级扩散**：优先掌握距离初始技能最近的一批技能，像水流一样均匀地向外扩散。             | **一路深入**：沿着某一条技能链一直学到尽头，遇到死胡同再回溯去学其他分支。        |
# BFS
``` python
import sys  
from collections import deque  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = [ [] for i in range(n + 1)]  
    q = deque()  
    visited = [False] * (n+ 1)  
  
    for i in range(1, n+1):  
        a, b = map(int, sys.stdin.readline().strip().split())  
  
        if a == 0 and b == 0:  
            q.append(i)  
            visited[i] = True  
        else:  
            arr[a].append(i)  
            arr[b].append(i)  
  
    ans = 0  
    while q:  
        x = q.popleft()  
        ans += 1  
  
        for y in arr[x]:  
            if not visited[y]:  
                visited[y] = True  
                q.append(y)  
  
    print(ans)  
  
if __name__ == '__main__':  
    main()
```
# DFS
``` python
import sys  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = [ [] for i in range(n + 1)]  
    # 将双端队列 deque 改为普通列表，作为栈（Stack）使用  
    stack = []  
    visited = [False] * (n + 1)  
  
    for i in range(1, n+1):  
        a, b = map(int, sys.stdin.readline().strip().split())  
  
        if a == 0 and b == 0:  
            stack.append(i)  
            visited[i] = True  
        else:  
            arr[a].append(i)  
            arr[b].append(i)  
  
    ans = 0  
    while stack:  
        # 将 popleft() 改为 pop()，弹出栈顶元素（后进先出，即 DFS）  
        x = stack.pop()  
        ans += 1  
  
        for y in arr[x]:  
            if not visited[y]:  
                visited[y] = True  
                stack.append(y)  
  
    print(ans)  
  
if __name__ == '__main__':  
    main()
```