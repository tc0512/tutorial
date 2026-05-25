# Sympy: 小学生都会用的强大符号计算库
## 1 安装sympy
所有设备都可以这样直接装
```bash
pip install sympy #-i https://pypi.tuna.tsinghua.edu.cn/simple/ #如果急着用的话
```
## 2 一般方程的解决
```python
from sympy import* #导入
x = Symbol("x") #未知数x
linear_with_one_unknown = 2*x - 4 #可以空格保证美观
print(solve(linear_with_one_unknown, x)) #solve(函数, 未知数), 函数值默认为0，返回列表
quadratic_with_one_unknown = x**2 - 5*x + 6
print(solve(quadratic_with_one_unknown, x)) #[2, 3], 即x1=2,x2=3
print(solve(x**5 - 1, x)) #五次方程也是可以的
print(solve(2*x**5 + 128*x**4 + 226*x**3 + 4*x**2 + 3*x - 222406, x)) #一堆CRootOf, 因为一般形式的一元五次方程没有通用根式解
print(nsolve(2*x**5 + 128*x**4 + 226*x**3 + 4*x**2 + 3*x - 222406, x, 10)) #数值近似解
```

## 3 线性方程组的解决
```python
from sympy import*
x, y, z = symbols("x y z") #同时定义多个未知数
eq1 = x + y - 5
eq2 = x - y - 1
print(solve([eq1, eq2], [x, y])) #{x: 3, y: 2}, 即x=3,y=2
eq1 = x + y + z - 6
eq2 = x + 2*y - 3*z + 4
eq3 = x - y + z - 2
print(solve([eq1, eq2, eq3], [x, y, z])) #{x: 1, y: 2, z: 3}
```

## 4 不等式的解决
