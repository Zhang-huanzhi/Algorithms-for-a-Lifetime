Python 的 for 循环在解释器层面是非常慢的，每次循环都要做类型检查、边界检查。而 Python 的内置函数底层全是高度优化的 C 语言代码。

凡是用 sum(), min(), max(), map(), str.join() 解决的操作，绝对不写 for 循环。