- **问题**：你在开头写了 from inspect import stack。在 Python 中，inspect.stack() 是一个用来获取**程序调用栈信息**（用于调试）的函数，它**不是**一个数据结构。
    
- **修正**：在 Python 中，实现“栈”直接用普通的**列表 []** 即可。