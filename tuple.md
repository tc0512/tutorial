# 元组: 不可变的列表
## 1  基本语法
```python
t1 = (1, 2, 3)
t2 = ("Hello", "world", "!")
t3 = (42,) #长度为1的元组要加逗号
print(isinstance(t3, tuple)) #True
t4 = 1, 2, 3 #自动识别为元组
t5 = () #空元组
t6 = (42)
print(isinstance(t6, tuple)) #False
print(isinstance(t6, int)) #True
```

## 2 常见操作
```python
t = (10, 20, 30, 40, 50)
# 基本上和列表一样, 但是不能改
print(t[0]) #10
print(t[1:3]) #(20, 30)
print(len(t)) #5
for item in t:
    print(item)
print(20 in t) #True

try:
    t[0] = 0
except TypeError:
    print("不能这么做")
```

## 3 注意事项
