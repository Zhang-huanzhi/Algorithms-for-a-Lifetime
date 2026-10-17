手写 8 个分支有两个致命缺点：

1. **极易写错**：只要复制粘贴时少改了一个 +1 或者 -1，代码直接 WA，且肉眼几乎查不出 BUG。
    
2. **毫无扩展性**：如果题目改成走日字（象棋马），或者在 3D 空间（走 26 个方向），手写分支会让人崩溃。

``` python
	if i - 1 > 0 and j - 1 > 0:  
	    if brr[i - 1][j - 1] == '#':  
	        crr[i][j] = '#'  
	if i - 1 > 0:  
	    if brr[i - 1][j] == '#':  
	        crr[i][j] = '#'  
	if i - 1 > 0 and j + 1 < w:  
	    if brr[i - 1][j + 1] == '#':  
	        crr[i][j] = '#'  
	if j - 1 > 0:  
	    if brr[i][j - 1] == '#':  
	        crr[i][j] = '#'  
	if j + 1 < w:  
	    if brr[i][j + 1] == '#':  
	        crr[i][j] = '#'  
	if i + 1 < h and j - 1 > 0:  
	    if brr[i + 1][j - 1] == '#':  
	        crr[i][j] = '#'  
	if i + 1 < h:  
	    if brr[i + 1][j] == '#':  
	        crr[i][j] = '#'  
	if i + 1 < h and j + 1 < w:  
	    if brr[i + 1][j + 1] == '#':  
	        crr[i][j] = '#'
```