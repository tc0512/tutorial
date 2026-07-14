# Python列表入门
## 1 基本语法
```python
<list name> = [a0, a1, a2,...]
```

## 2 列表的创建
```python
array = [1, 2, 3, 4, 5]
string = ["h", "e", "l", "l", "o"]
```

## 3 列表的访问
```python
array = [1, 2, 3, 4, 5]
print(array[0]) #从0开始数数, 0索引是第一项, 1索引是第二项, 以此类推
print(array[-1]) #负数也是可以的, 负几就是倒几

# 遍历
v = [6, 7, 8, 9, 10]
for i in v:
    print(i, end=" ")
print()
```

## 4 增删改查
```python
array = [1, 2, 3, 4, 5]
array.append(6) #在末尾加数字6
array.insert(1, 1.5) #在索引1插入数字1.5
array.remove(1) #删除第一个数字1 (这里只有一个, 千万不要以为是删所有匹配项) 
print(array.pop()) #[1.5, 2, 3, 4, 5] 删除最后一项并返回值
print(array) #[1.5, 2, 3, 4, 5, 6] 刚刚的pop不会更改原列表
del array[0] #删除指定索引
array[0] = 1
print(array) #[1, 3, 4, 5, 6]

#切片 (查) 
#基本语法: <list name>[start:end:step_length]
#start默认值: 0
#end默认值: 列表的长度
#step_length默认值: 1
#start, 索引与end的关系与for循环中i与start, end的关系类似
print(array[1:]) #[3, 4, 5, 6] 从索引1开始
print(array[1:4]) #[3, 4, 5] 从索引1开始, 不包含索引4
print(array[::2]) #[1, 4, 6] 隔一个
```

## 5 常用的列表操作
```python
array = [1, 2, 3, 4, 5]
print(len(array)) #长度5
print(max(array)) #最大值5
print(min(array)) #最小值1
print(sum(array)) #求和15
array.reverse() #反转并改变原列表
array.sort() #Tim Sort排序并改变原列表
print(3 in array) #是否存在
x0, x1, x2, x3, x4 = array #解包 (原列表还在) 
import random
random.shuffle(array) #随机打乱
print(array.is_sorted()) #是否已排序
unique = list(set([1, 2, 3, 3])) #去重
clean = [x for x in [1, None, 2, None, 3] if x is not None] #过滤掉None
```

## 6 列表推导式 (装X还省事) 
```python
# 传统写法
fx = []
for x in range(1, 6):
    fx.append(x**2)

# 列表推导式
fx = [x**2 for x in range(1, 6)]

# 取1-10所有奇数
fx = [i for i in range(1, 11) if i%2==1]
```

## 7 列表的拷贝
```python
a = [1, 2, 3]
b = a #错误示范: 重新赋值
b[2] = 4
print(a) #[1, 2, 4]

b = a.copy() #正确示范: 使用copy函数

# 二维数组 (矩阵) 
A = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
import copy
B = copy.deepcopy(A)
B[0][0] = 0
print(A, B) #[[1, 2, 3], [4, 5, 6], [7, 8, 9]] [[0, 2, 3], [4, 5, 6], [7, 8, 9]]
```

## 8 列表的拼接
```python
array = [1, 2, 3, 4, 5]
array+=6 #[1, 2, 3, 4, 5, 6]
array+=[7, 8] #[1, 2, 3, 4, 5, 6, 7, 8]
```
