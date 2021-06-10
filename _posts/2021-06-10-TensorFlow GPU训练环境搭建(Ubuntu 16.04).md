---
layout:     post
title:      TensorFlow GPU 训练环境搭建
subtitle:   Linux环境为Ubuntu 16.04
date:       2021-06-10
author:
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - AI
    - 人工智能
---

>TensorFlow GPU（Ubuntu 16.04）训练环境搭建

## 1.安装NVIDIA显卡驱动

1. 禁用nouveau，在终端（Ctrl-Alt+T）输入：

```
sudo gedit /etc/modprobe.d/blacklist.conf
```

在最后一行添加：

```
blacklist nouveau
```

保存退出，在终端（Ctrl-Alt+T）执行命令：

```
sudo update-initramfs -u
重启之后执行
lsmod | grep nouveau #没有输出则说明配置成功
```

2. 安装驱动，Ctrl-Alt+F1进入命令行界面之后输入用户名和密码登录 ，找到驱动文件NVIDIA_xxx.run所在目录（默认为当前用户目录的Downloads目录下）并赋予该文件可执行权限，然后进行安装：

```
cd Downloads
sudo chmod a+x NVIDIA_xxx.run
sudo service lightdm stop #关闭图形界面
sudo ./xxx.run -no-nouveau-check -no-opengl-files
```

完成后重启。

## 2.安装NVIDIA CUDA Toolkit 9.0

1. Ctrl-Alt+F1进入命令行界面之后输入用户名和密码登录 ，找到CUDA9.0文件cuda_xxx.run所在目录（默认为当前用户目录的Downloads目录下）并赋予该文件可执行权限，然后进行安装：

```
cd Downloads
sudo chmod a+x cuda_xxx.run
sudo service lightdm stop #关闭图形界面
sudo ./xxx.run
```

注意：安装过程中当询问是否安装显卡驱动时选n，因为先前已安装完显卡驱动无需再进行安装。

2. 配置环境变量：

```
sudo service lightdm start #开启图形界面
```

登录系统，打开终端（Ctrl-Alt+T）：

```
sudo gedit /etc/profile
```

在最后添加：

```
export PATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

保存退出，立即生效：

```
source /etc/profile
```

完成后重启。

3. 验证CUDA，打开终端（Ctrl-Alt+T）输入：

```
nvcc -V
```

出现相应版本号信息则说明安装成功。

## 3.安装NVIDIA cuDNN 7.4

1.打开终端（Ctrl-Alt+T），找到cuDNN文件libcudnn7_xxx.deb、libcudnn7-dev_xxx.deb、libcudnn7-doc_xxx.deb所在目录（默认为当前用户目录的Downloads目录下），进行安装：

```
sudo dpkg -i libcudnn7_xxx.deb
sudo dpkg -i libcudnn7-dev_xxx.deb
sudo dpkg -i libcudnn7-doc_xxx.deb
```

若不报错则说明安装成功。

2. 验证cuDNN是否已安装并可以正常运行，复制cuDNN sample到当前用户目录下：

```
cp -r /usr/src/cudnn_samples_v7/ $HOME
```

进入cuDNN相应测试样本的路径：

```
cd $HOME/cudnn_samples_v7/mnistCUDNN
```

编译该测试样本：

```
make clean && make
```

运行该测试样本：

```
./mnistCUDNN
```

若cuDNN安装并可正常运行则会出现：

```
Test passed!
```
