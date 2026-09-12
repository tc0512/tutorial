# Scipy: 数值计算界的万金油
## 1 安装
### Windows64位/Linux64位/MacOs64位
```bash
pip install scipy
```
### Termux on Android
```bash
pkg install python-scipy
```
iOS: a-shell自带

## 2 基本功能
⚠️注意: 本节内容可能令人不适, 请谨慎浏览
### 线代
```python
import numpy as np
from scipy import linalg

A = np.array([[1, 2], [3, 4]])

linalg.det(A)         # 行列式
linalg.inv(A)         # 逆矩阵
linalg.eig(A)         # 特征值/特征向量
linalg.svd(A)         # SVD
linalg.lu(A)          # LU 分解
linalg.cholesky(A.T@A) # Cholesky 分解
```
### 函数最值求解
```python
from scipy import optimize

# 最小化
def f(x):
    return (x[0] - 1)**2 + (x[1] - 2.5)**2

res = optimize.minimize(f, [0, 0], method='BFGS')
print(res.x)  # [1.  2.5]

# 求根
root = optimize.root(lambda x: x**2 - 4, 1)
print(root.x)  # [2.]

# 曲线拟合
def model(x, a, b):
    return a * x + b

xdata = np.linspace(0, 10, 50)
ydata = 2 * xdata + 1 + np.random.normal(0, 0.5, 50)
popt, _ = optimize.curve_fit(model, xdata, ydata)
print(popt)  # [2., 1.]
```
### 微积分
```python
from scipy import integrate

# 定积分
result, error = integrate.quad(lambda x: x**2, 0, 1)
print(result)  # ∫(0,1) x^2 dx=1/3 

# 二重积分
result, error = integrate.dblquad(lambda y, x: x*y, 0, 1, 0, 1)
print(result)  # ∫(0,1) ∫(0,1) xy dy dx=1/4

# 解 ODE
def dy_dt(y, t):
    return -y

sol = integrate.solve_ivp(dy_dt, [0, 5], [1], t_eval=np.linspace(0, 5, 100))
```
### 插值
```python
from scipy import interpolate

x = np.linspace(0, 10, 10)
y = np.sin(x)

f = interpolate.interp1d(x, y, kind='cubic')
y_new = f(np.linspace(0, 10, 100))
```
### 统计学
```python
from scipy import stats

# 正态分布
stats.norm.cdf(0)              # 0.5
stats.norm.pdf(0)              # 0.3989
stats.norm.ppf(0.975)          # 1.96

# 假设检验
t_stat, p_val = stats.ttest_ind([1, 2, 3], [4, 5, 6])

# 线性回归
slope, intercept, r, p, se = stats.linregress([1, 2, 3], [2, 4, 6])
```
### 信号处理
```python
from scipy import signal

# 滤波器设计
b, a = signal.butter(4, 0.1, 'low')
filtered = signal.filtfilt(b, a, data)

# 卷积
signal.convolve([1, 2, 3], [0, 1, 0.5])

# 找峰值
peaks, _ = signal.find_peaks(data)
```
### 稀疏矩阵
```python
from scipy import sparse
import numpy as np

# 创建稀疏矩阵
row = np.array([0, 0, 1, 2])
col = np.array([0, 2, 2, 0])
data = np.array([1, 2, 3, 4])

mat = sparse.csr_matrix((data, (row, col)), shape=(3, 3))

# 稀疏矩阵乘法
mat @ mat.T
```
### 空间算法
```python
from scipy import spatial

# 距离矩阵
points = np.array([[0, 0], [1, 0], [0, 1]])
spatial.distance.cdist(points, points)

# KD 树
tree = spatial.KDTree(points)
tree.query([0.5, 0.5])  # 最近邻
```
### 特殊函数
```python
from scipy import special

special.gamma(5)       # 24.0
special.beta(2, 3)     # 0.0833
special.erf(1)         # 0.8427
special.jv(0, 1)       # 贝塞尔函数
```
### 傅里叶
```python
from scipy import fft

x = np.array([1, 2, 3, 4])
X = fft.fft(x)         # 傅里叶变换
x_recovered = fft.ifft(X)
```

## 3 注意事项
1. scipy导入需约0.5秒, 请耐心等待
2. scipy安装体积略大, 尽量在急着用或感觉numpy不够用时安装
