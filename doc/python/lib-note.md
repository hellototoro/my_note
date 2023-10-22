# lib 笔记

## 目录

- [1. NumPy](#1-numpy)
  - [1.1 concatenate 函数](#11-concatenate-函数)
- [2. tqdm](#2-tqdm)

## 1. NumPy

### 1.1 concatenate 函数

NumPy的`numpy.concatenate`函数用于沿着现有轴连接数组序列。以下是一个示例：

```python
import numpy as np

# 创建两个数组
arr1 = np.array([[1, 2], [3, 4]])
arr2 = np.array([[5, 6]])

# 沿着第一个轴连接数组
result = np.concatenate((arr1, arr2), axis=0)

print(result)
```

输出：

```python
array([[1, 2],
       [3, 4],
       [5, 6]])
```

在这个例子中，我们创建了两个数组`arr1`和`arr2`，然后使用`np.concatenate`函数将它们沿着第一个轴连接起来，生成了一个新的数组`result`。

## 2. tqdm

tqdm库是一个常用的Python进度条库，可以在Python长循环中添加一个进度提示信息，用户只需要封装任意的迭代器。

直接在for循环中使用tqdm，如下所示：

```python
from tqdm import tqdm
for i in tqdm(range(1000)):
    time.sleep(.01) #进度条每0.1s前进一次，总时间为1000*0.1=100s
```

为进度条设置描述、手动控制进度和自定义进度条显示信息。例如：

```python
from tqdm import tqdm, trange
for i in trange(1000, desc="Processing", unit='batch'):
    time.sleep(.01) #进度条每0.1s前进一次，总时间为1000*0.1=100s
```

可以使用tqdm的write方法来输出信息。例如：

```python
from tqdm import tqdm, trange
for i in trange(1000):
    tqdm.write(f"Processing {i}")
    time.sleep(.01) #进度条每0.1s前进一次，总时间为1000*0.1=100s
```
