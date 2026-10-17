``` python
import itertools  
import math  
import sys  
from collections import deque, defaultdict, Counter  
  
def main():  
  
  
    t = int(sys.stdin.readline().strip())  
  
    def f(s):  
        st = []  
  
        for char in s:  
  
            if char == ')':  
  
                if len(st) >= 3 and st[-1] == 'x' and st[-2] == 'x' and st[-3] == '(':  
                    st.pop()  
                    st.pop()  
                    st.pop()  
  
                    st.append('x')  
                    st.append('x')  
                else:  
                    st.append(')')  
            else:  
                st.append(char)  
  
        return st  
  
    for _ in range(t):  
        s1 = sys.stdin.readline().strip()  
        s2 = sys.stdin.readline().strip()  
  
        st1 = f(s1)  
        st2 = f(s2)  
  
        if st1 == st2:  
            print("Yes")  
        else:  
            print("No")  
  
  
  
if __name__ == '__main__':  
    main()
```