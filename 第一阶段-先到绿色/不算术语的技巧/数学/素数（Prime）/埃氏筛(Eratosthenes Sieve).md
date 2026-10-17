[找素数的3种方法 试除法 埃氏筛 线性筛 动画演示过程 noi大纲数论基础_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV11epre3E5K/?spm_id_from=333.337.search-card.all.click&vd_source=79ca3da41a540ffd5bee721903882381)

**如果一个数是素数，那它所有的倍数（2倍、3倍、4倍...）都必然是合数**


---

``` python
def eratosthenes_sieve(n):  
    is_prime = [True] * (n + 1)  
    is_prime[0] = is_prime[1] = False  
    for i in range(2, int(n ** 0.5) + 1):  
        if is_prime[i]:  
            for j in range(i * i, n + 1, i):  
                is_prime[j] = False  
  
    primes = [i for i, val in enumerate(is_prime) if val]  
  
    return primes
```