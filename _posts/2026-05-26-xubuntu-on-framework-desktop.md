---
layout: post
title: "Xubuntu On Framework Desktop"
date: 2026-05-26
---

Before Install
==============

Windows Configuration
---------------------

To dual boot with Windows the following change is recommended

Control Panel > Power Options > Choose what the power buttons do > Change settings that are currently unavailable > Uncheck Turn on fast startup

After Install
=============

Fix WiFi
--------

I have an Intel 9260 WiFi card and had to do the following

(Not sure if the iwlwifi.conf change is necessary, but the grub change definitely is)

~~~
sudo nano /etc/modprobe.d/iwlwifi.conf
~~~

Add the following

~~~
options iwlwifi enable_ini=0
options iwlwifi power_save=0
options iwlmvm power_scheme=1
~~~

~~~
sudo update-initramfs -u
~~~

~~~
sudo nano /etc/default/grub
~~~

Modify the line starting with GRUB_CMDLINE_LINUX_DEFAULT to the following

~~~
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash pcie_port_pm=off"
~~~

~~~
sudo update-grub
~~~

Reboot

Remove Snap
-----------

Credit [Pjotr](https://easylinuxtipsproject.blogspot.com/p/xubuntu.html?m=1)

Remove the Snap infrastructure

~~~
sudo apt purge snapd
~~~

Create a preferences file to prevent future Snaps

~~~
sudo touch /etc/apt/preferences.d/nosnap.pref
~~~

~~~
sudo nano /etc/apt/preferences.d/nosnap.pref
~~~

Populate it with the following

~~~
Package: snapd
Pin: release a=*
Pin-Priority: -10
~~~

Install Firefox

~~~
sudo add-apt-repository ppa:mozillateam/ppa
~~~

~~~
sudo apt update
~~~

~~~
sudo apt install firefox-esr
~~~

Install OBS
-----------

~~~
sudo add-apt-repository ppa:obsproject/obs-studio
~~~

~~~
sudo apt update
~~~

~~~
sudo apt install obs-studio
~~~

Allow SMB Access Through Thunar
-------------------------------

~~~
sudo apt install gvfs-backends smbclient
~~~

Install VLC
-------------------------------

~~~
sudo apt install vlc
~~~

Install FFmpeg
-------------------------------

~~~
sudo apt install ffmpeg
~~~
Install DaVinci Resolve
-------------------------------

Download DaVinci_Resolve_X.X.X_Linux.zip

Download makeresolvedeb_X.X.X_multi.sh.tar.gz

Using DaVinci_Resolve_X.X.X_Linux.run from DaVinci_Resolve_X.X.X_Linux.zip and makeresolvedeb_X.X.X_multi.sh from makeresolvedeb_X.X.X_multi.sh.tar.gz create the deb file

~~~
./makeresolvedeb_X.X.X_multi.sh DaVinci_Resolve_X.X.X_Linux.run
~~~

Install from the deb file

~~~
sudo apt install ./davinci-resolve_X.X.X-mrdX.X.X_amd64.deb
~~~

Update groups, not sure if this change is necessary

~~~
sudo usermod -a -G render $USER
sudo usermod -a -G video $USER
~~~

Edit amdgpu.conf, not sure if this change is necessary

~~~
sudo nano /etc/modprobe.d/amdgpu.conf
~~~

Add the following

~~~
options ttm pages_limit=6400000
options ttm pages_pool_size=6400000
~~~

~~~
sudo update-initramfs -u
~~~

Edit environment, not sure if this change is necessary

~~~
sudo nano /etc/environment
~~~

Add the following line to the end of the file

~~~
HSA_OVERRIDE_GFX_VERSION=11.0.0
~~~

Download amdgpu-install_X.X.X-X_all.deb

Install from the deb file

~~~
sudo apt install ./amdgpu-install_X.X.X-X_all.deb
~~~

Install rocm drivers

~~~
sudo amdgpu-install --no-dkms --usecase=rocm,opencl --opencl=rocr
~~~

Fix path

~~~
echo "/opt/rocm/core-7.13/lib/opencl/libamdocl64.so" | sudo tee /etc/OpenCL/vendors/amdocl64.icd
~~~

Disable open source rust driver

~~~
sudo mkdir -p /etc/OpenCL/vendors/disabled
~~~

~~~
sudo mv /etc/OpenCL/vendors/rusticl.icd /etc/OpenCL/vendors/disabled/
~~~
