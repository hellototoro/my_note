# 多个软件版本共存

## 以gcc为例

### 1、安装需要的gcc版本

### 2、查看当前系统所有的gcc版本

```bash
ls /usr/bin/gcc*
```

### 3、给各个版本确定优先级，数字越大优先级越高

这里设置的110和90可以随意取，我的电脑有一个gcc11.0和gcc9.0，所以选了这两个数字。

```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-x 110
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-x 90
```

### 4、更新你的设置即可

```bash
sudo update-alternatives --config gcc
```
