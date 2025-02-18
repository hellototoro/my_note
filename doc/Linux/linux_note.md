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

3. 如果没有 authorized_keys 文件，则新建一个

```bash
cd .ssh
touch authorized_keys && chmod 600 authorized_keys
```

4. 最后将 id_ed25519.pub 追加到 authorized_keys 中

```bash
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys
```

### 2.3 清除指定名称的RSA（即删除 `~/.ssh/known_hosts` 里面指定的主机）

```bash
ssh-keygen -R hostname
```

## 3. 更换镜像源

### 3.1 linux镜像源

Ubuntu:

```bash
sudo cp -a /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i "s@http://.*archive.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
```

docker:

```bash
#在更换源之前，安装https的支持
apt install --reinstall ca-certificates
```

## 4. 常用设置

### 4.1 安装 oh-my-bash

项目地址[https://github.com/ohmybash/oh-my-bash](https://github.com/ohmybash/oh-my-bash)

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

设置.bashrc
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

### 4.2 设置代理

```bash
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"

unset http_proxy
unset https_proxy
```

### 4.3 安装mdns

```bash
# 1. 安装软件
sudo apt install avahi-daemon libnss-mdns

# 2. 启动服务
sudo systemctl enable --now avahi-daemon

# 3. 查看配置
cat /etc/nsswitch.conf
# now, make sure that `mdns4_minimal [NOTFOUND=return]` exists and goes after `files`
hosts:          files mdns4_minimal [NOTFOUND=return] dns

```

此时在局域网内就可以通过`hostname.local`代替ip进行远程登录等功能

## 5. 常用技巧

### 5.1 设置别名

```bash
alias ll='ls -l'
```

### 5.2 清理空间

```bash
rm -rf ~/.cache/*
rm -rf vscode*
rm -rf .vscode*

condactivate
conda env list | grep -v "^#" | awk '{print $1}' | xargs -I {} conda remove --name {} --all -y
conda clean -all -y
conda deactivate

sudo apt clean
sudo apt remove --purge package-name -y
sudo apt autoremove -y
sudo apt update
sudo apt upgrade -y
```

### 5.3 恢复默认环境变量

```bash
source /etc/environment
```

## 6. 添加PPA

[!什么是 Ubuntu PPA 以及如何安装它？](https://geekflare.com/ubuntu-ppa/)

## 7. 内核编译与安装

### 7.1 wsl2

1. 安装生成依赖项：
   `$ sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev`
2. 使用 WSL2 内核配置构建内核：
   `$ make KCONFIG_CONFIG=Microsoft/config-wsl`

安装内核模块和头文件
`sudo make modules_install headers_install`

将内核映像复制到 Windows 文件系统：
`cp arch/x86/boot/bzImage /mnt/c/`

### 7.2 SBC

##### 设置交叉编译环境（以Orangepi 5为例）

- 下载工具链（根据官方要求下载对应的工具版本）

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/armbian-releases/_toolchain/gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu.tar.xz

tar xf gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu.tar.xz

# 将其添加到环境变量中，例如：
export PATH=$PATH:/path/to/gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu/bin

# 或者
export CROSS_COMPILE=/path/to/gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu/bin/aarch64-linux-gnu-
```

- 下载kernel源码

```bash
# 5.10版本
git clone --depth=1 -b orange-pi-5.10-rk35xx https://github.com/orangepi-xunlong/linux-orangepi

# 6.1版本
git clone --depth=1 -b orange-pi-6.1-rk35xx https://github.com/orangepi-xunlong/linux-orangepi

tar zxf orange-pi-<kernel version>-rk35xx.tar.gz
```

- 编译

```bash
# 设置默认配置
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- rockchip_linux_defconfig

# 编译所有内容
make -j16 ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu-

# 或者单独编译
# 编译Image
make -j16 ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- Image

# 编译DTS
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- dtbs

# 编译modules
make -j16 ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- modules
```

- 树外编译模块

```bash
# 设置默认配置
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- rockchip_linux_defconfig

make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- oldconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- prepare
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- scripts
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- modules_prepare

```
