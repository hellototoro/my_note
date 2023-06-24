# python 笔记

## 1. 修改镜像源

### 1.1 单次有效

```bash
pip install model_name -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 1.2 永久有效

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

其他可选镜像源地址：

```html
https://mirrors.aliyun.com/pypi/simple
```
