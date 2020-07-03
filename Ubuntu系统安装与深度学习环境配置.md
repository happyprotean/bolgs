# Ubuntu系统安装与深度学习环境配置

## Ubuntu系统安装

- 官网下载ubuntu的iso文件，按照官网介绍制作启动盘，[官网下载](https://ubuntu.com/download/alternative-downloads)，[参考1](https://www.jianshu.com/p/38e6be8efecf)，[参考2](https://blog.csdn.net/discoverer100/article/details/94148635)

- 插入U盘，开机按F11选择启动项为U盘（需要将boot改为UEFI引导）

- Try ubuntu，进去之后选择install ubuntu

- 不要选择删除或者和window共存，选择其他安装

- 分区：需要将/boot所在分区修改为安装启动引导器的设备，其中/boot为启动引导盘，200-500M；swap为虚拟交换内存，为电脑内存的2倍；/为系统盘，所有软件的安装位置；/home为其他盘，用于存储文件

| 分区  | 大小 | 类型     | 文件 |
| ----- | ---- | -------- | ---- |
| /boot | 500M | 逻辑分区 | Ex4  |
| swap  | 30G  | 逻辑分区 | swap |
| /     | 150G | 主分区   | Ex4  |
| /home | 300G | 主分区   | Ex4  |

- 等待安装完成，重启，拔出U盘

## Ubuntu系统基本设置

- 设置用户用户密码  `sudo passwd root`

- 进入root用户 `su` 然后退出 `exit`

- 更改系统的源
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
vim /etc/apt/sources.list
# 从 mirrors.aliyun.com复制ubuntu 18.04的设置
```

- 更新系统软件 
```
sudo apt update
sudo apt upgrade
```

- **禁止内核更新**：一定要禁止内核更新，不然新内核与驱动不匹配，导致驱动不可用  [参考1](https://blog.csdn.net/weixin_40522162/article/details/80302735#commentBox) [参考2](https://www.cnblogs.com/yangzhaon/p/12911751.html)
```
dpkg --get-selections |grep linux-image     # 显示所有的内核
dpkg --get-selections |grep linux
uname -r    # 当前所有内核

sudo apt-mark hold linux-image-5.3.0-62--generic      # 禁止内核更新
sudo apt-mark hold linux-headers-5.3.0-62-generic
```

- 设置-区域与语言管理：管理已安装语言，然后更新

- 安装chrome浏览器，官网下载，默认下载到 /下载 文件夹 `sudo dpkg -i 安装包`

- 安装Typora：[官网](https://www.typora.io/#)

- 屏幕的调整：[参考](https://blog.csdn.net/u012207345/article/details/76283834)
```
Xrandr --output DP-1 --primary --auto
Xrandr --output HDMI-1 --right-of  DP-1 --auto
```



## 深度学习环境配置

- nvidia显卡驱动安装：安装的是440，[参考](https://blog.csdn.net/weixin_41863685/article/details/80303963)

```
sudo apt-get purge nvidia*
sudo vim /etc/modprobe.d/blacklist-nouveau.conf
	blacklist nouveau
	options nouveau modeset=0
sudo update-initramfs -u

sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
ubuntu-drivers devices
sudo apt-get install nvidia-driver-440
sudo reboot
```

- 安装依赖库 `sudo apt-get install freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev`
- 降低gcc版本

```
g++ --version
sudo apt-get install gcc-4.8
sudo apt-get install g++-4.8

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 50
```

- 下载cuda 10.0文件 [run文件](https://developer.nvidia.com/cuda-10.0-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal)，[安装参考](https://blog.csdn.net/liangjiubujiu/article/details/90522899)

```
sudo sh cuda_10.0.130_410.48_linux.run
```

- 安装cudnn7.4.1  [参考](https://blog.csdn.net/wanzhen4330/article/details/81699769)

```
tar -zxvf cudnn-10.0-linux-x64-v7.4.1.5.tgz

sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h
 sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

- 安装anaconda，官网下载速度较慢 [官网下载地址](https://www.anaconda.com/products/individual#linux)，[清华源](https://blog.csdn.net/u013431916/article/details/88980649)

`sudo bash Anaconda3-2020.02-Linux-x86_64.sh`

- 修改pip源：[参考](https://blog.csdn.net/w5688414/article/details/82832292)

```
cd ~
mkdir .pip
sudo vim .pip/pip.conf

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = mirrors.aliyun.com
```

- 安装tensorflow和keras  `pip install tensorflow-gpu==1.14.0`

```
pip install tensorflow-gpu==1.14.0
pip install keras
```

- 测试gpu是否可用：可用 [参考](https://blog.csdn.net/lidichengfo0412/article/details/103370476)

```
import tensorflow as tf
tf.test.gpu_device_name()

from tensorflow.python.client import device_lib 
device_lib.list_local_devices()

import tensorflow as tf   # 需要设置gpu的显存使用率，不然会报错
from keras.backend.tensorflow_backend import set_session
config = tf.ConfigProto()
config.gpu_options.allocator_type = 'BFC' #A "Best-fit with coalescing" algorithm, simplified from a version of dlmalloc.
config.gpu_options.per_process_gpu_memory_fraction = 0.3
config.gpu_options.allow_growth = True
set_session(tf.Session(config=config)) 
```

- 安装pytorch 1.2：[官网安装方式](https://pytorch.org/get-started/previous-versions/)

`pip install torch===1.2.0 torchvision===0.4.0 -f https://download.pytorch.org/whl/torch_stable.html`

```
import torch
torch.__version__
torch.cuda.is_available()
torch.cuda.device_count()
torch.cuda.get_device_name(0)
```

- 安装pycharm  [官网](https://www.jetbrains.com/pycharm/download/#section=linux)，[安装参考](https://blog.csdn.net/qq_15192373/article/details/81091278)

- 安装git `sudo apt-get install git`