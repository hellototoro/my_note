# Linux笔记

## 1. 多个软件版本共存

### 以gcc为例

### 1.1 安装需要的gcc版本

### 1.2 查看当前系统所有的gcc版本

```bash
ls /usr/bin/gcc*
```

### 1.3 给各个版本确定优先级，数字越大优先级越高

这里设置的110和90可以随意取，我的电脑有一个gcc11.0和gcc9.0，所以选了这两个数字。

```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-x 110
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-x 90
```

### 1.4 更新你的设置即可

```bash
sudo update-alternatives --config gcc
```

## 2. ssh免密登录

### 2.1 生成ssh密钥

先查看是否已经生成ssh密钥

```bash
ls -al ~/.ssh
```

如果没有，则根据ssh/ssh.md生成ssh密钥

### 2.2 上传公钥到服务器

Linux系统：
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub  user@xx.xx.xx.xx
```

ssh-copy-id 命令：
把本地主机的公钥复制到远程主机的 authorized_keys 文件中，并设置合适的权限。
其中 -i 参数：指定公钥文件，如果不传入 -i 参数，ssh-copy-id 使用默认的 ~/.ssh/identity.pub 作为默认公钥。

Windows系统：
1. 将 id_ed25519.pub 复制到需要远程登陆的主机上
```powershell
cd C:\Users\<username>\.ssh\
scp .\id_ed25519.pub user@xx.xx.xx.xx:~
```

2. 在~目录下查看是否有 .ssh 文件夹及 authorized_keys 文件
如果没有 .ssh 文件夹，则新建一个
```bash
mkdir .ssh
chmod 700 .ssh
```

如果没有 authorized_keys 文件，则新建一个
```bash
cd .ssh
touch authorized_keys
chmod 600 authorized_keys
```

最后将 id_ed25519.pub 追加到 authorized_keys 中
```bash
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys
```

## 3. 更换镜像源

### 3.1 linux镜像源

Ubuntu:

```bash
sudo cp -a /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i "s@http://.*archive.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
```

## 4. 常用设置

### 4.1 安装 oh-my-bash

项目地址[https://github.com/ohmybash/oh-my-bash](https://github.com/ohmybash/oh-my-bash)
```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```
启用插件
```bash
plugins=(git bundler osx rake ruby)
```
选择主题
```bash
OSH_THEME="powerline"

OSH_THEME="agnoster" # (this is one of the fancy ones)
# you might need to install a special Powerline font on your console's host for this to work
# see https://github.com/ohmybash/oh-my-bash/wiki/Themes#agnoster

OSH_THEME="random" # (...please let it be pie... please be some pie..)
```

## 5. 常用技巧

### 5.1 设置别名

```bash
alias ll='ls -l'
```
