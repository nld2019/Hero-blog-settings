---
title: adb error 常用解决方法
date: 2022-06-17 13:34:25
tags: Misc
---
## 1，adb shell 提示：error: insufficient permissions
> user@HP:~/work/tcumtk2731\$ adb shell
> error: insufficient permissions for device
> user@HP:~/work/tcumtk2731\$
> 
> 1) adb devices 确认设备情况
> user@HP:~/work/tcumtk2731\$ adb devices
> * daemon not running. starting it now on port 5037 *
> * daemon started successfully *
> List of devices attached
> ????????????	no permissions
> 
> 2) 找到adb 的所在路径
> user@HP:~/work/tcumtk2731\$ which adb
> /usr/bin/adb
> user@HP:~/work/tcumtk2731\$ cd /usr/bin/
> 
> 3) 查看adb的权限
> user@HP:/usr/bin\$ ls -l adb
> -rwxr-xr-x 1 root root 160912 4月   1  2016 adb
> user@HP:/usr/bin\$
> 
> 4)添加设置用户ID权限位
> root@HP:/usr/bin# chmod u+s adb
> root@HP:/usr/bin# ls -l adb
> -rwsr-xr-x 1 root root 160912 4月   1  2016 adb
> root@HP:/usr/bin#
> 
> 5）重启 adb server
> user@HP:/usr/bin\$ adb kill-server
> user@HP:/usr/bin\$
> user@HP:/usr/bin\$ adb start-server
> * daemon not running. starting it now on port 5037 *
> * daemon started successfully *
> user@HP:/usr/bin\$
> 
> 6)查看adb设备情况
> user@HP:/usr/bin\$ adb devices
> List of devices attached
> B6FAEFAC	device
> 
> user@HP:/usr/bin\$

## 2，遇到问题no permissions (verify udev rules)
> user@HP:/usr/bin\$ adb devices
> List of devices attached
> B6FAEFAC	no permissions (verify udev rules); see [http://developer.android.com/tools/device.html]
> 
> 1）查看当前设备的 vendor ID 和 product ID
> user@HP:/etc/udev/rules.d\$ lsusb
> Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
> Bus 001 Device 006: ID 18d1:4ee7 Google Inc.
> Bus 001 Device 003: ID 413c:2105 Dell Computer Corp. Model L100 Keyboard
> Bus 001 Device 002: ID 03f0:1f4a Hewlett-Packard
> Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
> user@HP:/etc/udev/rules.d\$
> user@HP:/etc/udev/rules.d\$
> 
> 2） 将上面的两个ID添加到51-android.rules文件中
> user@HP:/etc/udev/rules.d\$ cat 51-android.rules
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee7", MODE="0666",GROUP="plugdev"
> user@HP:/etc/udev/rules.d\$



## 3，执行adb shell 时报错 error: device not found.
> 1）使用lsusb 查看usb设备
> Bus 002 Device 002: ID 8087:8000 Intel Corp. 
> Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
> Bus 001 Device 002: ID 8087:8008 Intel Corp. 
> Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
> Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
> Bus 003 Device 002: ID 0461:4e04 Primax Electronics, Ltd 
> Bus 003 Device 011: ID 2717:9039  //我的设备
> Bus 003 Device 003: ID 17ef:6019 Lenovo 
> Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
> 
> 若不知道哪个是你的设备,可以拔掉后lsusb,然后对比即可.
> 
> 2）创建adb_usb.ini文件，写入id
> 
> 在home下寻找.android目录,在此目录下新建一个文件adb_usb.ini.
> 
> echo 0x2717> ~/.android/adb_usb.ini
> 
> 3）添加权限
> sudo vim /etc/udev/rules.d/70-android.rules
> 
> 加入设备的相关ID
> SUBSYSTEM=="usb", ATTRS{idVendor}=="2717", ATTRS{idProduct}=="9039",MODE="0666"
> 
> 4）重启USB服务
> \$sudo chmod a+rx /etc/udev/rules.d/70-android.rules
> \$sudo service udev restart
> 
> 5）重启adb服务，adb devices有设备说明adb安装成功
> \$adb kill-server
> 
> \$sudo adb start-server
> 
> \$adb devices
> List of devices attached
> 5cb00b6 device

