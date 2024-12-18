# PyTorch 笔记

## 杂记

### 1 CUDA版本管理

1.1 PyTorch 运行时的 CUDA 版本

```python
# Pytorch 实际使用的运行时的 cuda 目录
import torch.utils.cpp_extension
torch.utils.cpp_extension.CUDA_HOME
# 编译该 Pytorch release 版本时使用的 cuda 版本
import torch
torch.version.cuda 
```

1.2 PyTorch 寻找可用 CUDA 的过程：

- 1. 环境变量 CUDA_HOME 或 CUDA_PATH

- 2. /usr/local/cuda

- 3. which nvcc 的上级上级目录（which nvcc 会在环境变量PATH中找）

- 4. 如果上述都不存在，则 torch.utils.cpp_extension.CUDA_HOME 为 None ，会使用 conda 安装的 cudatoolkit ，其路径为 cudart 库文件目录的上级目录（此时可能是通过 conda 安装的 cudatoolkit ，一般直接用 conda install cudatoolkit ，就是在这里搜索到 cuda 库的）。

pytorch 只需要 cuda 的运行时

cudatoolkit 包含 pytorch 所需要的运行时，还能编译cuda代码

调用哪个 cuda 库要看生成 tensorflow / pytorch 库的时候，设置的链接库寻找目录，以 pytorch 为例，项目根目录下的 setup.py 中指定了链接库的搜索目录，其中 cuda 的根目录 CUDA_HOME在 tool.setup_helpers.cuda 中有获取逻辑，大概过程是：

0. ~/.bashrc下指定的CUDA_HOME
1. 默认 cuda 安装目录 /usr/local/cuda
2. 如默认目录不存在（例如安装原生 cuda 到其他自定义位置），那么搜索 nvcc 所在的目录
3. 如果 nvcc 不存在，那么直接寻找 cudart 库文件目录（此时可能是通过 conda 安装的 cudatoolkit，一般直接用 conda install cudatoolkit，就是在这里搜索到 cuda 库的），库文件目录的上级目录就作为 CUDA_HOME。
4. 如果最终未能得到 CUDA_HOME，那么生成的 pytorch 将不使用 CUDA。

### 1 使用CPU

一般只用CPU进行推理：
例如：加载模型
GPU版：

```python
torch.load('xxxNet.pth')
```

CPU版：

```python
torch.load('xxxNet.pth', map_location='cpu')
```
