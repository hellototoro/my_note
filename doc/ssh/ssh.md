# ssh 笔记

## 1、生成ssh密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

注意：如果你使用的是不支持 Ed25519 算法的旧系统，请使用以下命令：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

生成时如使用默认配置，直接一路回车即可。

## 2、查看公钥

默认路径：
Windows：C:\Users\huang\.ssh

```powershell
cat C:\Users\huang\.ssh\id_ed25519.pub
```

Linux：~/.ssh

```bash
cat ~/.ssh/id_ed25519.pub
```
