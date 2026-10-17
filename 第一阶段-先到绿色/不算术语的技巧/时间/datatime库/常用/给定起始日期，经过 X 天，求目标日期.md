#### 1. 核心模型：datetime + timedelta

这是处理“天数偏移”的最强组合。


``` python
from datetime import datetime, timedelta

def solve():
    # 假设输入：2023 1 1 经过 100 天后的日期
    year, month, day = 2023, 1, 1
    days_to_add = 100
    
    # 构造 datetime 对象
    start_date = datetime(year, month, day)
    
    # 直接加 timedelta，这会自动处理所有的跨月、跨年、闰年
    result_date = start_date + timedelta(days=days_to_add)
    
    # 格式化输出（CSP 题目通常要求严格格式，比如 2023 04 11）
    print(f"{result_date.year} {result_date.month} {result_date.day}")

# 这个逻辑能秒杀 90% 的日期类大模拟
```