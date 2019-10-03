#  Ubuntu install note

## Disk type verification
If your disk type is GPT, we remomend you to use UEFI model to install Ubuntu.

## Create disk partitions
I recommend to mount partitions according to the following strategy.
* 30%: /
* 10%: Swap space
* 60%: /home

NO OTHER PARTITIONS NEEDED!

## Time different
If you install ubunut along with Windows, you have to change your time to avoid two systems have different time.

``` shell
$ sudo apt-get install ntpdate
$ sudo ntpdate time.windows.com
$ sudo hwclock --localtime --systohc
```

## Change PIP package source
The official pip source is always stay up tp date, however, it may be slow to download packages from this server, so for some cases, change to Tsinghua server to help to download packges quickly.
``` shell
$ pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple
```
## GPU Drive install
File preparation:
NVIDIA Drives

Software request:
cmake, gcc

Steps:

Move and backpack the original open source drivers, since it will not allow other drivers to be installed
``` shell
$ sudo mv /lib/modules/$(uname -r)/kernel/drivers/gpu/drm/nouveau/nouveau.ko /lib/modules/$(uname -r)/kernel/drivers/gpu/drm/nouveau/nouveau.ko.org

$ sudo update-initramfs -u
```
Restart system and after system start up, type the following commands in the fold where you download you driver. We use Driver.run to represent the driver.
``` shell
$ sudo chmod +x Driver.run
$ sudo ./Driver.run --no-x-check --no-nouveau-check --no-opengl-files
```
Check out the default options while installing, after installation, use the following commands to check GPU status.
``` shell
$ nvidia-smi
```

## CUDA Toolkit installation

Some computational work needs Compute Unified Device Architecture library(CUDA) such as Tensorflow, Pytorch and Caffe. To install CUDA Runtime ennvironment, please follow these instructions below.

Download CUDA Runtime library from offical source, we recommend to download .run file
``` shell
https://developer.nvidia.com/cuda-downloads
```
Then, use sudo sh cuda.run(replace with the file name) to install, notice if you have already installed Nvidia driver, DO NOT choose to install driver while installing cuda.

## CUDNN installation
The NVIDIA CUDA Deep Neural Network library (cuDNN) is a GPU-accelerated library of primitives for deep neural networks. We are usually aqucired to install CUDNN for better performance when training. The following instructions will guide you to install CUDNN.

In order to download cuDNN, ensure you are registered for the NVIDIA Developer Program in this link.
```
https://developer.nvidia.com/cudnn
```
Then choose the installation method that meets your environment needs.

Now navigate to directory where the cuDNN Tar file place.Use the following commands.

``` shell
$ tar -xzvf cudnn-9.0-linux-x64-v7.tgz
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## Use Onedrive on Ubuntu 
If you want to use Onedrive in Ubuntu, please check out this repository
``` 
https://github.com/skilion/onedrive
```
For convinience, we collect steps on how to install Onedrive in few conmands.
``` shell
$ sudo apt install libcurl4-openssl-dev libsqlite3-dev
$ sudo snap install --classic dmd && sudo snap install --classic dub

$ git clone https://github.com/skilion/onedrive.git && cd onedrive &&make && sudo make install
```

now onedrive has installed, you need to intialize onedrive first before you use it, just type onedrive in your command line and follow the instructions.

To enable the auto-sync feature, you need to use the following command.
```
$ systemctl --user enable onedrive
$ systemctl --user start onedrive
```
All files will sync in ~/Onedrive after you have done.
