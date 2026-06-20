# 信息中考python速通教程
本教程也可供初学者使用

## 1 `print`语句
### 基本语法
```python
print(<format type>"<string>", **kwargs)
```
### Hello world
```python
print("Hello world!")
```
### 常见的转义字符
| 转义字符 | 含义 |
| ----- | ------ |
| `\n` | 换行 |
| `\r` | 光标返回行首 |
| `\t` | 制表符 |
### 常见的字符串格式化
| 格式化符号 | 用法 |
| ----- | ------ |
| `f"<string>"` | 调用变量 |
| `r"<string>"` | 忽略转义字符 |
| `%d` `%s` | C风格 |
### 常见的尾置参数
| 参数 | 用法 |
| ------ | ------ |
| `end="<string>"` | 以某字符串为结尾且取消本次换行 |
| `sep="<string>"` | 分割多个参数 |
### 示例代码
```python
print("这是第一行")
print("这是第二行\n这是第三行")
print(r"这个转义字符\n会被忽略")
print("这一串文本", end="")
print("在同一行上")
print("这是新的一行")
```
运行效果: 
```text
这是第一行
这是第二行
这是第三行
这个转义字符\n会被忽略
这一串文本在同一行上
这是新的一行
```
## 2 运算符
运算符对照表
| 运算符 | 含义 |
| ------ | ------ |
| `+` | 加法 |
| `-` | 减法 |
| `*` | 乘法 |
| `/` | 除法 |
| `**` | 幂运算 |
| `//` | 除法向下取整 |
| `%` | 除法取余数 |
| `>` | 大于 |
| `<` | 小于 |
| `>=` | 大于等于 |
| `<=` | 小于等于 |
| `==` | 等于 (`if-else`中使用)  |
| `and` | 与 |
| `or` | 或 |
| `not` | 非 |
示例代码: 
```python
print(1+1)
print(2-2)
print(3*3)
print(4/4)
print(2**3)
print(5//2)
print(5%2)
print(5>2)
print(5<3)
print(6>=8)
print(7<=7)
print(10==10)
print(2+2==4 and 0/2==0.0)
```
运行效果: 
```text
2
0
9
1.0
8
2
1
True
False
False
True
True
```

## 3 变量
### 基本语法
```python
<var name> = <value>
```
### 变量命名规则
1. 变量名可以包含大小写字母, 数字, 下划线和汉字
2. 不能以数字开头
3. 一般地, 当变量有多个单词时, 通常用下划线连接
4. 上述情况也可以使用驼峰命名法, 例如`MyFirstVar`
5. 一般地, 第一个字符是下划线的变量是私有变量
示例代码: 
```python
a = 1 #int
b = 0.1 #float
s = "hello" #string
p1 = (0, 0) #tuple
lst = [1, 2, 3, 4, 5] #list
bl = True #bool
```
### 变量的赋值特征: 可以重复多次赋任意值, 以最后一次赋值为准
### 变量的调用
```python
a = 1
b = 2
print("a+b =", a+b) #方法1: 逗号
c = 0.5
d = 0.2
e = c-d
print(f"c-d={e}") #方法2: f-string
x = 2
y = 3
res = x**y
print("x^y=%d^%d=%d" % (x, y, res)) #方法3: C风格
```
补充: C风格格式化占位符对照表
| 字母 | 含义 |
| ------ | ------ |
| `%d` `%i` | 整数 |
| `%u` | 正整数 |
| `%o` | 八进制整数 |
| `%x` `%X` | 十六进制整数小写/大写 |
| `%f` `%F` | 小数 |
| `%e` `%E` | 科学记数法小写/大写 |
| `%g` `%G` | 自动选择`%f`或`%e %E` |
| `%c` | 单字 |
| `%s` | 字符串 |
| `%r` | 通用 |
| `%a` | 通用 (乱码概率低)  |
| `%%` | 输出单个百分号 |
### 强制转型
```python
pi = 3.14
print(int(pi)) #砍掉小数部分
s = "1"
print(int(s)) #整数1
print(float(s)) #1.0
```

## 4 `input`语句
### 基本语法
```python
<var name> = <type>(input("<tip word>"))
```
### 示例代码
```python
username = input("Enter your user name:") #字符串不需要转型
password = input("Enter your password:")
print("You are successfully authenticated.")
print("Your name:", username)
print("Your password:", password)

verify_code = int(input("Please Enter your verify code:")) #只能输入整数, 否则报错
print("The verify code you given:", verify_code)

a = float(input("Please enter the first number:")) #浮点数 (数字通用) 
b = float(input("Please enter the second number:"))
print("a + b =", a, "+", b, "=", a+b)
```
运行效果: 
```text
Enter your user name:example
Enter your password:abcd1234
You are successfully authenticated.
Your name: example
Your password: abcd1234
Please Enter your verify code:908856
The verify code you given:908856
Please enter the first number:1.0
Please enter the second number:2.5
a + b = 1.0 + 2.5 = 3.5
```

## 5 分支语句
### 基本语法
```python
if <condition>:
    <code>
elif <another condition>:
    <code>
else:
    <code>
```
示例代码: 
```python
a = 6
if a>=9:
    a = 3
print(a) #6

b = 8
if a>b:
    b = b-3
b = 7
print(b) #7

c = 3
if a>=c:
    c = c-6
else:
    c = c+3
print(c) #-3

d = 9
if c==d:
    d = 2
else:
    d = 3
d = 8
print(d) #8
```

## 6 `for`循环
### 基本语法
```python
for <range var> in range(<start>, <end>, <step length>):
    <code>
```
示例代码: 
```python
for i in range(10): #一个参数: 省略start和step_length
    print(i) #0到9十个数一行一个
print("\n", end="") #输出换行

for i in range(10):
    print(i, end=" ") #0 1 2 3 4 5 6 7 8 9 

for i in range(1, 11): #两个参数: 省略step_length
    print(i) #1到10一行一个

for i in range(1, 12, 2): #三个参数
    print(i) #1-11内所有奇数
```
### 注意事项
1. 注意缩进, 能用Tab坚决不用空格, 避免`TabError`
2. `start` <= `range_var` < `end`
3. start默认值: 0, step\_length默认值: 1

## 7 中考赚分技巧 (差生必看)
`print()`是啥就是啥, 调试时期用处多。
看见`\n`快换行, 中考仅考一转义。
看见`end`别换行, 空格过去是正道。
加减和数学一样, 乘是星号除斜杠。
向下取整俩斜杠, 取余数用百分号。
不等号参照数学, 逻辑等于双等号。
与或非按英语翻, 变量命名很简单。
名字等号变量值, 字母数字下划线。
汉字也能来命名, 数字绝不能开头。
多词下划线连接, 驼峰命名也可以。
下划线首是私有, 多次赋值随便搞。
最后一次为基准, 强制转型并不难。
`int()` `float()` 直接套, 输入`input`别吓到。
变量名等号后加, 类型`input`提示词。
字符串不用转型, 数字要`int()`或`float()`。
`float()`所有数字通用, 不知类型时推荐。
`if-else`别紧张, 逻辑如果和否则。
正常逻辑来解题, 二次赋值莫犹豫。
`for`循环相对较难, 实则只是纸老虎。
起终步长三兄弟, 一定要分清记牢。
单参数只有终点, 此时起0步长1。
双参数含起终点, 前有等号后没有。
遇三参数莫慌神, 也就加了个步长。
算钱认题目要求, 合适转型往外套。
细胞分裂选乘方, 存储换算选`<unit>=`。
求和`var=i+var`, 初值为0要记牢。
阶乘只是变乘号, 初值是1莫迷糊。
上述口诀必记住, 及格万岁万事了。
