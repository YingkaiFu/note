#  Ubuntu install note

## Disk type verification
If your disk type is GPT, we remomend you to use UEFI model to install Ubuntu.
## Time different
If you install ubunut along with Windows, you have to change your time to avoid two systems have different time.

``` shell
sudo apt-get install ntpdate
sudo ntpdate time.windows.com
sudo hwclock --localtime --systohc
```

## Change PIP package source
The official pip source is always stay up tp date, however, it may be slow to download packages from this server, so for some cases, change to Tsinghua server to help to download packges quickly.
```
pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple
```
## GPU Drive install
File preparation:
NVIDIA Drives

Software request:
cmake, gcc

Steps:

Move and backpack the original open source drivers, since it will not allow other drivers to be installed
```
sudo mv /lib/modules/$(uname -r)/kernel/drivers/gpu/drm/nouveau/nouveau.ko /lib/modules/$(uname -r)/kernel/drivers/gpu/drm/nouveau/nouveau.ko.org
```
Restart system and after system start up, type the following commands in the fold where you download you driver. We use Driver.run to represent the driver.
```
sudo chmod +x Driver.run
sudo ./Driver.run --no-x-check --no-nouveau-check --no-opengl-files
```
Type yes for all the options, after installation, use the following commands to check GPU status.
```
nvidia-smi
```
## Use Onedrive on Ubuntu 
If you want to use Onedrive in Ubuntu, please check out this repository
``` 
https://github.com/skilion/onedrive
```
For convinience, we collect steps on how to install Onedrive in few conmands.
``` 
sudo apt install libcurl4-openssl-dev libsqlite3-dev
sudo snap install --classic dmd && sudo snap install --classic dub

git clone https://github.com/skilion/onedrive.git && cd onedrive &&make && sudo make install
```

now onedrive has installed, you need to intialize onedrive first before you use it, just type onedrive in your command line and follow the instructions.

To enable the auto-sync feature, you need to use the following command.
```
systemctl --user enable onedrive
systemctl --user start onedrive
```
After that, all your files has store in ~/Onedrive
