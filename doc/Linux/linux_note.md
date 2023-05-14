# Linux笔记

## 多个软件版本共存

### 以gcc为例

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

## ssh免密登录

### 1、生成ssh密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

注意：如果你使用的是不支持 Ed25519 算法的旧系统，请使用以下命令：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

生成时如使用默认配置，直接一路回车即可。

### 2、上传公钥到服务器

ssh-copy-id -i ~/.ssh/id_rsa.pub  user@xx.xx.xx.xx

ssh-copy-id 命令：
把本地主机的公钥复制到远程主机的 authorized_keys 文件中，并设置合适的权限。
其中 -i 参数：指定公钥文件，如果不传入 -i 参数，ssh-copy-id 使用默认的 ~/.ssh/identity.pub 作为默认公钥。

## 更换镜像源

### 1、linux镜像源


## 常用技巧
### 1. 设置别名
```bash
alias ll='ls -l'
```
