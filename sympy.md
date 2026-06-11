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
```python
from sympy import*
x = Symbol("x")
ine = (2*x > 4)
print(solve(ine, x)) #(2 < x) & (x < oo) 人话x>2, oo是无穷大
print(solveset(ine, x, S.Reals)) #Interval.open(2, oo) 此处是两边都没有等号, 一般情况下, 哪open哪就没有等号, 都有等号时返回Interval(a, b)
```

## 5 合并同类项
```python
from sympy import*
x, y = symbols("x y")
print(simplify(2*x + 2*x)) #4*x
print(simplify((x + y)**2)) #(x + y)**2, 一般情况下, 化简f(x)^n使用expand函数
print(expand((x + y)**2)) #x**2 + 2*x*y + y**2 ✅
```

## 6 分式计算
```python
from sympy import*
a = Symbol("a")
ex = (a + 1)*(1 - a)/((1 - a)**2)
print(simplify(ex)) #(-a - 1)/(a - 1)
equation = (a + 1)/(a - 1) - 3
print(solve(equation, a)) #[2]
```

## 7 根式化简
```python
from sympy import*
print(simplify(sqrt(2) + sqrt(2))) #2*sqrt(2)
print(sqrtdenest(sqrt(2 + sqrt(3)))) #(sqrt(6) + sqrt(2))/2 双层根号化简只能用sqrtdenest
```
## 8 几何计算
```python
from sympy import*
O = Point(0, 0) #点
A = Point(3, 4)
print(O.distance(A)) #两点之间距离 输出5
print(O.midpoint(A)) #中点 输出Point(3/2, 2)
print(Line(O, A).equation()) #两点拟合一次函数y=4/3*x
l1 = Line(O, A) #直线
print(l1.slope()) #4/3 一次函数kx+b的k, 专业用语叫斜率
l2 = Line(Point(1, 1), Point(2, 2))
print(l1.intersection(l2)) #交点, 返回列表
print(l1.is_perpendicular(l2)) #True 垂直
print(l1.is_parallel(l2)) #False 平行
c = Circle(Point(0, 0), 1) #以(0, 0)为圆心, 半径为1的圆
print(c.area) #pi 面积
print(c.circumference) #2*pi 周长
print(c.intersection(l2)) #与直线l2的交点, 也返回列表
triangle = Polygon(Point(0, 0), Point(3, 0), Point(0, 4)) #多边形
print(triangle.area) #面积6
print(triangle.perimeter) #周长12
rectangle = Polygon(Point(0, 0), Point(3, 0), Point(3, 4), Point(0, 4)) #长方形也是可以的
```

## 9 导数
```python
from sympy import*
x = Symbol("x")
y = 2*x-2
print(diff(y, x)) #一阶导2
print(diff(y, x, 2)) #二阶导0
y = Symbol("y")
z = x*y
print(diff(z, x)) #偏导数y
```

## 10 注意事项
1. sympy在解超定方程组时可能会输出很长的一坨解
2. 不等式解集提取只能用solveset, left是最小, right是最大, L左R右
3. sympy导入可能需要1-2秒时间, 请耐心等待
4. 不要化简2000项以上的根式, 别问我怎么知道的
