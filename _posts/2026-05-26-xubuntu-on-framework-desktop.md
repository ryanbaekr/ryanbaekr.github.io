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
