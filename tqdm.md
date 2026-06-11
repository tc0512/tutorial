# Tqdm: 让你知道程序跑了多少, 告别被急死
## 1 安装
```bash
pip install tqdm #-i https://pypi.tuna.tsinghua.edu.cn/simple/
```

## 2 基础用法: range函数外直接套
```python
from tqdm import tqdm
total = 0
for i in tqdm(range(1, 100000001)):
    total = total+i
print(total)
```

## 3 手动控制进度条 (最灵活) 
```python
from tqdm import tqdm

# while循环
total = 0
i = 1
pbar = tqdm(total=100000000)
while i<=100000000:
    total = total+i
    i = i+1
    pbar.update(1)
print(total)

# 双层for循环
pbar = tqdm(total=100000000)
for x in range(10000):
    for y in range(10000):
        z = x*y #亿些计算
        pbar.update(1)
```

## 4 自定义描述文字
```python
from tqdm import tqdm
import time

for i in tqdm(range(100), desc="下载中"):
    time.sleep(0.01)

# 动态更新描述
pbar = tqdm(range(100), desc="处理")
for i in pbar:
    time.sleep(0.01)
    pbar.set_description(f"进度 {i+1}/100")
```

## 5 单位换算
```python
from tqdm import tqdm
import time

# 处理 1e6 个元素
for i in tqdm(range(1000000), unit="个"):
    pass

# 下载文件（字节）
for i in tqdm(range(1024*1024), unit="B", unit_scale=True):
    pass

# 自定义单位
for i in tqdm(range(100), unit="条", unit_scale=False):
    time.sleep(0.01)
```

## 6 不同风格
```python
from tqdm import tqdm
import time

# 默认风格
for i in tqdm(range(100), desc="默认"):
    time.sleep(0.01)

# ASCII 风格（适合没有 Unicode 的环境）
for i in tqdm(range(100), desc="ASCII", ascii=True):
    time.sleep(0.01)

# 简洁风格
for i in tqdm(range(100), desc="简洁", ncols=50):
    time.sleep(0.01)
```

## 7 嵌套进度条
```python
from tqdm import tqdm
import time

for i in tqdm(range(3), desc="外层"):
    for j in tqdm(range(100), desc="内层", leave=False):
        time.sleep(0.001)
```

## 8 注意事项
1. 进度条可能拖慢运行速度
2. IPython, Jupyter notebook等伪终端需使用特殊方法
