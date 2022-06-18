---
title: 常用Linux-Android命令和快捷键
date: 2022-06-18 18:17:21
tags: Misc
---

// Ubuntu快捷键
// Ctrl+Alt+T    打开终端
// ctrl+shift+t  打开终端 同一窗口中
// Ctrl+D        关闭终端
// CTRL+A        光标移至命令行首
// Ctrl+e        光标移至命令行尾
// Ctrl+i        查找命令行中历史命令
// shift + UP    终端向上翻页
// shift + Down  终端向下翻页
// ============================================================
// Windows快捷键
// 
// Ctrl + Shift + F      win10自带输入法切换简繁体
// ============================================================
// Linux 常用命令
// ifconfig -a       查看网口信息
// 
// 
// Paste this into a shell to add Vysor to your apt repositories and then install it.
// (echo 'deb [arch=amd64, trusted=yes] https://nuts.vysor.io/apt ./' | sudo tee /etc/apt/sources.list.d/vysor.list) && sudo apt update && sudo apt install vysor
// 
// lsmod             查看驱动load信息
// 
// insmod mlan.ko    加载驱动
// 
// insmod moal.ko mod_para=nxp/wifi_mod_para.conf
// 
// dmesg             打印内核log
// 
// cat /sys/kernel/debug/mmc0/clock
// cat /sys/kernel/debug/mmc0/ios
// 
// 
// cat /proc/mwlan/adapter0/config    查看NXP驱动文件内容
// 
// ps -eo pid,lstart,cmd | grep wpa   查看特定进程的信息及启动参数
// 
// w    查看当前活动的用户
// 
// id   查看用户的详细信息
// 
// 
// dumpsys bluetooth_manager | head -5           查看当前BT的状态
// bt_vendor.conf                                用于配置BT的参数
// 
// 
// 查看国家码
// iw reg get/set
// 
// 新建会话：tmux new -s session-name
// 查看会话：tmux ls
// 进入会话：tmux a -t session-name
// 断开会话：tmux detach
// 关闭会话：tmux kill-session -t session-name
// 
// 
// 1. nohup
// 
// \$ nohup ./mytest &
// nohup sh issue_i_simulate_script.sh &
// 
// 2. setsid
// 
// \$ setsid ./mytest &
// 
// #PS:以上2种方式是对未运行的程序操作，如果进程已经在后台运行，如果系统支持的话，可以使用disown命令:
// 
// $ ./mytest &
// [1] 2222
// \$ disown -h %1
// 
// 3. screen
// \$ screen -dmS mytest
// 
// \$ screen -r mytest
// 
// 4. subshell（实质为子进程执行方式,可以参见subshell的执行方式，通常为fork）
// \$ (./mytest &)
// 
// 5. trap
// 
// 使用trap命令可以忽略或者回复系统信号对当前脚本的影响。
// 忽略：
// \$ trap "" SIGHUP SIGINT 或trap "" 1 2
// 恢复：
// \$ trap SIGHUP SIGINT 或 trap : 1 2
// 
// 
// 上下分屏：ctrl + b 再按 "
// 左右分屏：ctrl + b 再按 %
// 切换屏幕：ctrl + b 再按 o
// 关闭分屏：ctrl + b 再按 x，也可以直接exit
// 上下左右分屏切换： ctrl + b 再按空格键
// 
// 
// wpa_supplicant -Dwext -imlan0 -c/etc/wpa_supplicant.conf -ddd -B      wpa_supplicant的一个成功启动实例
// 
// wpa_cli -imlan0 -p/var/run/wpa_supplicant                             wpa_cli的一个成功应用实例
// 
// > scan                                                                发起扫描
// 
// > scan_results                                                        获取扫描结果
// 
// > flush                                                               清除当前的netid
// 
// > add_network                                                         新增network
// 
// > set_network 0 ssid "test"                                           选择需要连接的SSID
// 
// > set_network 0 psk "11111111hack3"                                   选择的SSID对应的密码
// 
// > select_network 0                                                    发起连接
// 
// > status                                                              获取当前的状态
// 
// > q                                                                   退出wpa_cli的交互命令行
// 
// 
// 
// 
// apt-file search /netlink/genl/genl.h      通过apt在线查找包括文件的库
// 
// 
// 制作combo 固件的方法
// cat env/cmd7_combo_with_crc.bin w9097d/w9097d-serial.bin w9097o/w9097o.bin > w9097_combo_p222.bin
// 
// cat env/cmd7_combo_with_crc.bin w8997d/w8997d-serial.bin w8997o/w8997o.bin > w8997_combo.bin
// 
// 
// 配置路由
// =====================================================================
// ifconfig wlan0 up
// sleep 1
// ifconfig wlan0 192.168.0.12
// sleep 1
// ip route add 192.168.0.0/24 via 192.168.0.12 dev wlan0 table local
// 
// 命令行
// 修改主机名     vim /etc/hostname
// 
// sudo dmidecode --type memory    查看本机的内存类型
// 
// free -mh   查看磁盘空间
// 
// df -h 查看磁盘空间
// 
// du -sh 查看当前目录的大小
// 
// modinfo 模块名     用于显示模块信息
// modinfo mii
// 
// find .|xargs grep -r "IBM"   查找目录下的所有文件中是否含有某个字符串
// 
// 查询当前目录下所有文件中是否有某个文件
// find ./ -name "NCLog.h"
// 
// uname -a    查看系统版本信息
// uname -r    显示内核版本信息
// 
// grep -nir "wlan.ko" 查看带关键字的文件
// 
// lsmod 查看系统中加载的模块
// 
// insmod 加载模块到内核
// 
// rmmod 卸载模块,卸载模块如果遇到失败，需要尝试 down 掉 网口。
// 
// ssh username@ip_address   远程登陆
// 
// scp -r root@10.6.159.147:/opt/soft/test /opt/soft/   从远程到本地
// 
// scp -r /opt/soft/test root@10.6.159.147:/opt/soft/scptest   从本地到远程
// 
// mount -o remount, rw /
// adb remount   上面一条无法解决，用这个
// mount -o remount rw /system  解决目录/system为只读的问题
// mount -t vfat /dev/sda1 /mnt 将U盘挂载到20项目HU上
// 
// 2>&1 > /dev/null
// 
// <command> 2>&1 | tee <logfile>            重定向到文件并输出标准输出；
// <command>  2> logfile | cat - logfile     重定向到文件并输出标准输出；
// 
// rename 's/p2p/sta/' *  批量修改文件名
// 
// diff -Naur 原文件 新修改文件 > rivision.patch
// 
// patch -p0 < rivision.patch 对原文件打上新修改的补丁
// patch -p0 -R < rivision.patch 对打过补丁的原文件卸载补丁
// -p 参数后面的0 表示 去掉补丁文件中第0个 /的路径
// 1表示 去掉 补丁文件中第1个 /的路径，实际上，在生成普通文件的同级目录中 -p 后面的参数都是0
// 
// find  ./ -type f -exec dos2unix {} \;   修改提交中 CRLF would be replaced by LF
// 
// wget https://fossies.org/linux/privat/lcov-1.13.tar.gz  终端下载文件
// 
// 
// #UUU tool 
// .\uuu.exe -b emmc_all imx-boot-imx8qxpmek-sd.bin-flash imx-image-multimedia-imx8qxpc0mek.wic
// 
// 
// while true; do date; sleep 1; done;
// 
// 
// 
// 
// netcfg wlan0 dhcp   分配IP
// busybox ifconfig   串口中查看设备的网址信息
// busybox udhcpc -i wlan0                  DHCP分配IP
// busybox ifconfig wlan0 192.168.43.114    分配IP
// 如果不具备动态分配IP的条件，可以将测试手机设置静态IP进行测试
// 
// getprop  获取运行状态信息
// 
// 
// echo 0 > /proc/sys/kernel/printk   关内核打印
// $ cat /proc/sys/kernel/printk
//     7       4       1       7
// 
//           7                       4                      1                           7
// console_loglevel、default_message_loglevel、minimum_console_loglevel、default_console_loglevel
// 
// // This two ID can confirm the specific chip vendor.    search the driver sorce code of these two ID
// cat /sys/bus/sdio/devices/sdio:0001:1/device            // lookup sdio device id
// cat /sys/bus/sdio/devices/sdio:0001:1/vendor            // vendor id
// 
// cat /proc/kmsg   实时查看内核log
// 
// logcat -c
// logcfg --showc
// logcat -s connectmanager nhpnp -v time  过滤处所需信息
// 
// 
// ##Get information about thread on linux.
// (gdb) info thread    this can get information about thread
// 
// ps -m -l [PID]     this can get information about thread
// 
// ps -T -l [PID]   this give some more information about thread
// 
// 
// 文本批量处理
// find ./ -type f -exec sed -i -E "s/NWlanLog::MsgOut(\(.*\));/MsgOutWlh\(\(TEXT\1\)\);/g" {} \;
// find ./ -type f -exec sed -i -E "s/MsgOutWlh\(\(TEXT\((\".*\"),(.*)\)\)\);/MsgOutWlh\(\(TEXT\(\1\),\2\)\);/g" {} \;
// 
// sed -i -E "s/NWlanLog::MsgOut(\(.*\));/MsgOutWlh\(\(TEXT\1\)\);/g"  DeviceLibCom/NEventWatcherCom/src/NWlanHandler/NWlanHandler.cpp
// sed -i -E "s/MsgOutWlh\(\(TEXT\((\".*\"),(.*)\)\)\);/MsgOutWlh\(\(TEXT\(\1\),\2\)\);/g" DeviceLibCom/NEventWatcherCom/src/NWlanHandler/NWlanHandler.cpp
// sed -i -E "s/.*MsgOutWlh\(\(TEXT\(\".*\[END\]\"\)\)\);//g"  NWlanDriverManager.cpp
// sed -i -E "s/MsgOutWlh\(\(TEXT\((\".*)\s+\[START\]\"\)\)\);/MsgOutWlh\(\(TEXT\(\1\"\)\)\);/g"  NWlanDevRequest.cpp 
// 
// cloc    统计代码的工具
// 
// sbust in Makefile    //将 文本中的 A 替换为 B
// 
// 通过网线连接ADB
// 1, adb connect IP
// 2, 1连接完成后，可以执行adb root, adb shell, 等adb的命令。
// 
// 
// 
// 1)    2.4G 40MHz support disabled (BUILT_DISABLE_2G_BAND_40MHZ_BW = 1) in FP92
// Could not transmit in 40MHz even with below driver commands to enable 40MHz, due to firmware would force the tx capability to 20MHz.
//           $ mlanutl mlan0 htcapinfo 0x05c20000 1
//                     $ mlanutl mlan0 httxcfg 0x62 1
//                     Could receive in 40MHz with “mlanutl mlan0 htcapinfo 0x05c20000 1” command to configure 40MHz in the htcapinfo.
// 
//                     2)    2.4G 40MHz support enabled (BUILT_DISABLE_2G_BAND_40MHZ_BW = 0) in FP92
//                     Both Tx/Rx could be in 40MHz with below commands
//                               $ mlanutl mlan0 httxcfg 0x62 1
//                               $ mlanutl mlan0 htcapinfo 0x05c20000 1
// Besides, FP92 of other chips (including 8997) also re-enables the 2.4G 40MHz support in firmware after firmware V16_xx_10_173 tag. Thanks!
// 
// 
// Android wifi tag
// logcat -s
// ConnectivityModuleConnector NetworkScoreService SystemServiceManager  WifiCountryCode WifiContext WifiOpenNetworkNotifier HalDevMgr WakeupController 
// WifiClientModeImpl WifiService WifiScanningService SystemServerTiming  WifiP2pService ConnectivityService WifiController WifiClientModeManager android_os_HwBinder
// WifiNl80211Manager hwservicemanager WifiManager android.hardware.wifi@1.0-service.droidlogic WifiVendorHal WifiP2pNative  WifiHAL wificond WifiNative
// SupplicantStaIfaceHal WifiScanRequestProxy WifiScanningService WifiScanner wpa_supplicant WifiCountryCode WifiDiags WifiActiveMod
// eWarden WifiNetworkFactory WifiConnectivityHelper WifiScoringParams WifiConfigStore Settings NetworkSecurityConfig system_server SQLiteDatabase WificondScannerImpl
// WifiHealthMonitor WifiAsyncChannel WifiTracker hostapd SoftApManager Tethering dnsmasq WifiActiveModeWarden HostapdHal 

