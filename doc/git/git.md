# git 笔记

## 1. 安装

Windows：[下载地址](https://git-scm.com/downloads)

Linux：

```bash
sudo apt update
sudo apt install git
```

## 2. 配置

### 2.1 设置个人信息

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

查看

```bash
git config user.name
git config user.email
git config --list
```

### 2.2 关联GitHub

#### 2.2.1 生成ssh密钥

先查看是否已经生成ssh密钥

```bash
ls -al ~/.ssh
```

如果没有，则根据ssh/ssh.md生成ssh密钥

#### 2.2.2 GitHub添加ssh key

打开设置
![打开设置](./resource/open_setting.png)

根据ssh/ssh.md查看公钥，并添加公钥(sshkey)
![添加ssh key](./resource/add_ssh_key.png)

#### 2.2.3 验证是否添加成功

```bash
ssh -T git@github.com
```

当看到如下内容时，表明添加成功

```bash
Hi <github id>! You've successfully authenticated, but GitHub does not provide shell access.
```

### 2.3 设置代理

```bash
# 设置代理
# 假设代理服务器地址为：127.0.0.1，端口为：7890
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890

# sock5代理
git config --global http.proxy socks5 127.0.0.1:7891
git config --global https.proxy socks5 127.0.0.1:7891

# 查看代理
git config --global --get http.proxy
git config --global --get https.proxy

#取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```


## 3. 使用

### 3.1 添加子仓库

本地仓库添加远程仓库作为子仓库

```bash
git submodule add https://github.com/xxx/xxx.git
```
