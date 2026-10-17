``` python
import sys  
  
  
def solve():  
  
    s = sys.stdin.readline().strip()  
  
    MOD = 998244353  
  
    dp = [0, 0, 0]  
    total = 0  
  
    # 建立字符到索引的映射  
    mapping = {'a': 0, 'b': 1, 'c': 2}  
  
    for char in s:  
        idx = mapping[char]  
  
        new_count = (total - dp[idx] + 1) % MOD  
  
        # 更新总数  
        total = (total + new_count) % MOD  
  
        # 更新以该字符结尾的序列统计值  
        dp[idx] = (dp[idx] + new_count) % MOD  
  
    print(total)  
  
  
if __name__ == '__main__':  
    solve()
```