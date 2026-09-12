# Numpy: 日下载超2000万的顶梁柱
## 1 安装
### Windows/Linux/MacOs64位
```bash
pip install numpy
```
### Termux on Android
```bash
pkg install python-numpy
```
iOS: AppStore安装a-shell (别装成迷你版! ) , numpy开箱即用
除iOS外, 32位设备建议放弃

## 2 基础操作
```python
import numpy as np

# 数组
a = np.array([1, 2, 3, 4, 5])

# 全零, 注意是(长, 宽)
z = np.zeros((3, 4))

# 全一
o = np.ones((2, 3))

# 单位矩阵, 括号里的是大小
i = np.eye(5)

# 等差序列
r = np.arange(0, 10, 2)   # [0, 2, 4, 6, 8]
l = np.linspace(0, 1, 5)  # [0, 0.25, 0.5, 0.75, 1]

# 随机数
rand = np.random.rand(3, 3)        # 0-1 均匀分布
randn = np.random.randn(3, 3)      # 标准正态分布
randint = np.random.randint(0, 10, (3, 3))  # 整数随机
```

## 3 数组属性
```python
import numpy as np

a = np.array([[1, 2, 3], [4, 5, 6]])

a.shape      # 形状(2, 3)
a.ndim       # 维度2
a.size       # 大小6(单位: 项)
a.dtype      # 每一项的数据类型int64
a.itemsize   # 每一项的大小8(单位: 字节)
```

## 4 索引与切片
```python
import numpy as np

a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# 切片 (和普通列表一样) 
a[2:5]       # [3, 4, 5]
a[:5]        # [1, 2, 3, 4, 5]
a[5:]        # [6, 7, 8, 9, 10]
a[::2]       # [1, 3, 5, 7, 9]

# 二维索引
b = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
b[0, 1]      # 2
b[1, :]      # [4, 5, 6]
b[:, 1]      # [2, 5, 8]

# 布尔索引 (直接取所有符合条件的数, 写列表推导式的功夫都省了) 
mask = (a > 5)
a[mask]      # [6, 7, 8, 9, 10]

# 花式索引 (选取特定行, 一维数组自动退化成项)
a[[0, 2, 4]] # [1, 3, 5]
```

## 5 数组运算
```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# 逐元素运算
a + b        # [5, 7, 9]
a - b        # [-3, -3, -3]
a * b        # [4, 10, 18]
a / b        # [0.25, 0.4, 0.5]
a ** 2       # [1, 4, 9]

# 矩阵乘法 (一维数组自动退化成点积) 
np.dot(a, b) # 32
a @ b        # 32

# 广播
a + 10       # [11, 12, 13]
a * 2        # [2, 4, 6]
```

## 6 常用函数
```python
import numpy as np

a = np.array([1, 2, 3, 4, 5])

np.sum(a)        # 求和15
np.mean(a)       # 平均数3.0
np.median(a)     # 中位数3.0
np.std(a)        # 标准差1.414
np.var(a)        # 方差2.0
np.min(a)        # 最小1
np.max(a)        # 最大5
np.argmin(a)     # 最小值所在位置的索引0
np.argmax(a)     # 最大值所在位置的索引4
np.sort(a)       # 排序[1, 2, 3, 4, 5]

# 矩阵运算
m = np.array([[1, 2], [3, 4]])
np.linalg.det(m)     # 行列式-2.0
np.linalg.inv(m)     # 逆矩阵[[-2, 1], [1.5, -0.5]], 如果想求伪逆用pinv
np.linalg.eig(m)     # 特征值
```

## 7 形状操作
```python
import numpy as np

a = np.array([[1, 2, 3], [4, 5, 6]])

a.reshape(3, 2)      # 转成特定形状[[1, 2], [3, 4], [5, 6]]
a.flatten()          # 转一维[1, 2, 3, 4, 5, 6]
a.T                  # 转置
np.concatenate([a, a])      # 把B接在A下面
np.vstack([a, a])           # 同上
np.hstack([a, a])           # 增广
```

## 8 常用技巧
```python
import numpy as np

# 条件筛选
a = np.array([1, 2, 3, 4, 5])
np.where(a > 3, a, 0)  # 不符合条件的数全部替换为特定值[0, 0, 0, 4, 5]

# 唯一值 (去重) 
np.unique([1, 2, 2, 3, 3, 4])  # [1, 2, 3, 4]

# 数组复制
b = a.copy()  # 深拷贝

# 保存/加载
np.save('data.npy', a)
a = np.load('data.npy')
```

## 9 实际应用 (手搓BFGS) 
⚠️注意: 本节内容可能令人不适, 请谨慎浏览
```python
import numpy as np

def _line_search(f, grad, x, p, c1=1e-4, alpha=1.0, rho=0.5):
    """简单 Armijo 线搜索"""
    while f(x + alpha * p) > f(x) + c1 * alpha * grad(x) @ p:
        alpha *= rho
    return alpha

def BFGS(f, grad, x0, max_iter=100, tol=1e-6):
    """
    手搓 BFGS
    f: 目标函数
    grad: 梯度函数
    x0: 初始点
    max_iter: 最大迭代次数
    tol: 收敛容差
    """
    x = np.array(x0, dtype=float)
    n = len(x)
    H = np.eye(n)  # 初始 Hessian 近似（单位矩阵）

    g = grad(x)

    for i in range(max_iter):
        # 搜索方向
        p = -H @ g

        # 线搜索（Armijo 条件）
        alpha = _line_search(f, grad, x, p)

        # 更新 x
        x_new = x + alpha * p

        # 更新梯度
        g_new = grad(x_new)

        # 计算 s 和 y
        s = x_new - x
        y = g_new - g

        # 检查收敛
        if np.linalg.norm(g_new) < tol:
            break

        # BFGS 更新 H
        rho = 1.0 / (y @ s)
        I = np.eye(n)
        H = (I - rho * np.outer(s, y)) @ H @ (I - rho * np.outer(y, s)) + rho * np.outer(s, s)

        x = x_new
        g = g_new

    return x, f(x), i + 1
```

## 10 注意事项
1. numpy导入较慢 (约0.2秒) 请耐心等待
2. `np.array`运算非常快, 进行大规模运算时可以不用普通列表
3. 超大规模计算向量化加速之前务必检查运行内存是否充足
