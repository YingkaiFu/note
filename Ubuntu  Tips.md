# Ubuntu  Tips

# 🦴installation

## 1. Disk type verification

If your disk type is GPT, we remomend you to use UEFI mode to install Ubuntu.

## 2. Create disk partitions

I recommend to creat partitions according to the following strategy. 
* 30%: / 
* 10%: Swap space 
* 60%: /home

**NO OTHER PARTITIONS NEEDED! These partitions are enough!**

## 3. Correct time discrepancy

If you install ubuntu along with Windows, you have to correct your time to avoid both systems have different time.

```bash
sudo apt-get install ntpdate
sudo ntpdate time.windows.com
sudo hwclock --localtime --systohc
```

# 👥Change PIP package source

The official pip source is always stay up to date, however, it may be slow to download packages from this server in some countries, so for some cases, change to Tsinghua server or aliyun server can help to download packges faster.

```bash
pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple # Tsinghua source
# or Aliyun source
pip install scrapy -i https://mirrors.aliyun.com/pypi/simple/
```

If you want to change the pip source permanently, you can edit ~/.pip/pip.conf and add the following text to this file:

```bash
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

However, mirror souce have some delay to synchronize with the main souce, packages installed in these way may have lower version.

# 🧩DL environment preparing

## 1. GPU Drive install

**File preparation**: NVIDIA Drives downloaded from here: [https://www.nvidia.cn/Download/index.aspx?lang=cn](https://www.nvidia.cn/Download/index.aspx?lang=cn)

**Software request**: cmake, gcc

1.1 Move and backpack the original open source drivers, since it will not allow other drivers to be installed

```bash
sudo mv /lib/modules/$(uname -r)/kernel/drivers/gpu/drm/nouveau/nouveau.ko /lib/modules/$(uname -r)/kernel/drivers/gpu/drm/nouveau/nouveau.ko.org

sudo update-initramfs -u
```

1.2 Restart system 

1.3 Type the following commands in the fold where you download you driver. We use Driver.run to represent the driver.

```bash
sudo chmod +x Driver.run
sudo ./Driver.run --no-x-check --no-nouveau-check --no-opengl-files
```

1.4 Check out the default options while installing, after installation, use the following commands to check GPU status.

```bash
nvidia-smi
```

## 2. CUDA Toolkit installation

Some computational work needs Compute Unified Device Architecture library(CUDA) such as Tensorflow, Pytorch and Caffe. To install CUDA Runtime ennvironment, please follow these instructions below.

Download CUDA Runtime library from offical source, we recommend to download .run file

```
https://developer.nvidia.com/cuda-downloads
```

Then, use sudo sh cuda.run(replace with the file name) to install, notice if you have already installed Nvidia driver, DO NOT choose to install driver while installing cuda.

## 3. CUDNN installation

The NVIDIA CUDA Deep Neural Network library (cuDNN) is a GPU-accelerated library of primitives for deep neural networks. We are usually aqucired to install CUDNN for better performance when training. The following instructions will guide you to install CUDNN.

In order to download cuDNN, ensure you are registered for the NVIDIA Developer Program in this link.

```
https://developer.nvidia.com/cudnn
```

Then choose the installation method that meets your environment needs.

Now navigate to directory where the cuDNN Tar file place.Use the following commands.

```bash
tar -xzvf cudnn-9.0-linux-x64-v7.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
# Install Nvidia-docker in Ubuntu

You need to download and install docker firstly according to the following commands:
``` bash
sudo apt-get update

sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

After installing docker in the computer, use the following command to make use of your GPU in docker container.

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker

# ☁️Use Onedrive on Ubuntu

If you want to use Onedrive in Ubuntu, please check out this repository

```
https://github.com/skilion/onedrive
```

For convinience, we collect steps on how to install Onedrive in few conmands.

```bash
sudo apt install libcurl4-openssl-dev libsqlite3-dev
sudo snap install --classic dmd && sudo snap install --classic dub

git clone https://github.com/skilion/onedrive.git && cd onedrive &&make && sudo make install
```

now onedrive has installed, you need to intialize onedrive first before you use it, just type onedrive in your command line and follow the instructions.

To enable the auto-sync feature, you need to use the following command.

```bash
systemctl --user enable onedrive
systemctl --user start onedrive
```

All files will sync in ~/Onedrive after you have done.

# 📗OpenCV Installation

**Precondition**:

```bash
# development tools
sudo apt-get install build-essential cmake unzip pkg-config 
# image and video io
sudo apt-get install libjpeg-dev libpng-dev libtiff-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
# GUI backend
sudo apt-get install libgtk-3-dev
# Mathematical optimization package
sudo apt-get install libatlas-base-dev gfortran
```

Download source code from the following URL

```bash
https://github.com/opencv/opencv
https://github.com/opencv/opencv_contrib
```

unpack the ziped files and start build using the following steps:

```bash
cd opencv 
mkdir build && cd build
cmake -D CMAKE_INSTALL_PREFIX=/usr/local \
-D CMAKE_BUILD_TYPE=Release \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-D BUILD_EXAMPLES=ON ..
make -j all
sudo make install
```

During building process, you may needs to download IPPICV if you have pool network, check the following link to get IPPICV.

```bash
https://github.com/opencv/opencv_3rdparty/tree/ippicv/master_20170822/ippicv
```
