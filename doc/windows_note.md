# windows note

## wsl 访问usb设备

### 软件安装
1、在Windows上安装usbipd，下载usbipd-win：[https://github.com/dorssel/usbipd-win/releases](https://github.com/dorssel/usbipd-win/releases)

2、在Linux上安装USBIP工具和硬件数据库：
```bash
sudo apt install linux-tools-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20
```

### Linux连接usb设备
1、在Windows上以管理员身份打开power shell，查看busid
```powershell
usbipd wsl list
```

2、附加一个usb设备到默认的Linux发行版，在power shell上执行以下命令：
```powershell
usbipd wsl attach --busid <busid>
```
此外还可以使用 --distribution <Linux distribution> 来指定发行版，查看Linux的发行版可以使用一下命令查看
```powershell
wsl --list
```

3、在Linux设备上可以使用以下命令查看：
```bash
lsusb
```

4、断开usb设备
在 WSL 中完成设备使用后，可物理断开 USB 设备，或者在管理员模式下从 PowerShell 运行此命令：
```powershell
usbipd wsl detach --busid <busid>
```
