# OpenCR

## 1. USB端口设置（on Linux）

```bash
wget https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/99-opencr-cdc.rules
sudo cp ./99-opencr-cdc.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules
sudo udevadm trigger
```

## 2. Arduino IDE设置（on Linux）

### 2.1. 编译器设置

在64位PC上需要安装32位的库

```bash
sudo apt update
sudo apt install libncurses5-dev:i386
```

到官网下载最新的 [Arduino IDE](https://www.arduino.cc/en/Main/Software) for Linux

### 2.2. 添加OpenCR board

After Arduino IDE is run, click File → Preferences in the top menu of the IDE. When the Preferences window appears, copy and paste following link to the Additional Boards Manager URLs textbox. (This step may take about 20 min.)

```http
https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json
```
