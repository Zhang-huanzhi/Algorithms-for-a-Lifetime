``` python
class UnionFind:
    def __init__(self, n):
        # 初始化：每个人各自为一个集合，代表节点是自己
        self.parent = list(range(n))
        # 【新增优化】：记录每个集合的大小，初始时每个点所在的集合大小为 1
        self.size = [1] * n  

    def find(self, x):
        # 【核心优化】：把你的“递归”改成了“while循环迭代”
        # 只要还没找到根节点（根节点的特征是 parent[x] == x），就一直往上找
        while self.parent[x] != x:
            # 路径减半（顺手优化）：让当前节点直接连到它的“爷爷”节点上
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        
        # 如果本来就在同一个集合，直接跳过（可用于判环）
        if root_x == root_y:
            return False
            
        # 【核心优化】：按秩（大小）合并
        # 原则：永远把“小树”挂到“大树”的下面，这样树才不会变成一根长刺
        if self.size[root_x] > self.size[root_y]:
            self.parent[root_y] = root_x          # root_y 这棵小树，认 root_x 做老大
            self.size[root_x] += self.size[root_y] # root_x 的地盘变大了
        else:
            self.parent[root_x] = root_y          # root_x 这棵小树，认 root_y 做老大
            self.size[root_y] += self.size[root_x] # root_y 的地盘变大了
            
        return True

    # 【新增功能】：获取某个节点所在集合的总人数（AtCoder 极高频考点）
    def get_size(self, x):
        return self.size[self.find(x)]
```