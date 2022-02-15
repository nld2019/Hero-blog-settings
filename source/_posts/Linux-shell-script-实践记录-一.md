---
title: Linux shell script 实践记录(一)
date: 2022-02-15 18:02:16
tags: 'Shell script'
---

本文展示一个Linux shell scpript实例。其中涉及的的一些Linux和shell相关的知识的如下：
1. shell script 首行语法
2. shell 的注释方法及script的代码规范一般形式
3. shell 条件语句
4. shell scprit中调用另外一个shell script.
5. while 循环
6. 获取系统参数并保存变量，变量计数
7. 输出重定向及文件名创建编号。
8. 终端断开不杀脚本的方法
9. Wi-Fi的连接操作等


主shell script.

```shell script
#!/bin/sh
##ISSUE i simulation script.
##Objects:Try to simulate the Wi-Fi interaction sequence to accelerate issue reproduce
#Use method:
#1. Make sure the Wi-Fi is enable when board power on.
#2. adb push issue-i-test-tool /data/test/
#3. adb shell 
#4. cd /data/test/
#5. nohup sh issue_i_simulate_script.sh &

#Stop Android framework
stop

rm -rf /data/dump_*
cp ./hostapd_ap0.conf /data/vendor/wifi/hostapd/hostapd_ap0.conf
chown wifi:wifi /data/vendor/wifi/wpa/wpa_supplicant.conf
chown wifi:wifi /data/vendor/wifi/hostapd/hostapd_ap0.conf

dmesg -C
ERR_NUM=0
NUM=0
if [ -e /data/test/kernel_log ]; then
   rm -rf /data/test/kernel_log/*
   echo "kernel_log dir exitst"
else
    mkdir -p /data/test/kernel_log/
fi

echo "######## statrt new simulation #########"

while true
do
    echo "######## statrt new simulation #########" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
    #Load 8997 driver
    #insmod mlan.ko
    #insmod moal.ko drv_mode=3 sta_name=wlan0 uap_name=ap cal_data=none cfg80211_wext=0xf fw_name=nxp/sdiouart_combo.bin
    
    #ifconfig wlan0 up
    
    ##Start wpa_supplicant
    #/vendor/bin/hw/wpa_supplicant -Dnl80211 -iwlan0 -c/data/vendor/wifi/wpa/wpa_supplicant.conf &
    #start wpa_supplicant
    wpa_cli -i wlan0 disconnect

    ##Set driver and FW debug level
    mlanutl wlan0 drvdbg 0x800FF
	sleep 1
	##Set driver and FW debug level
    mlanutl ap0 drvdbg 0x800FF
    #cat /proc/kmsg > ./kernel_log/kernel_log.txt &
    echo "Set drvdbg 0x800FF and capture kernel log." 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
    
    ##Start scan every 10 seconds
    sh /data/test/cycle_scan.sh $NUM &
    
    ##Start AP
    ##hostapd hostap.conf
    echo "------START SOFT AP-----" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
    start hostapd
    
    
    media_state=$(cat /proc/mwlan/adapter0/wlan0/info  | grep -i media_state | cut -b 14-22)
    echo "$media_state" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
    
    NUM_CONNECT=0
    ##Connect remote ap specified by ssid
    while true
    do
        #wpa_cli -i wlan0 remove_net all
        #wpa_cli -i wlan0 flush
        #wpa_cli -i wlan0 add_network
        #wpa_cli -i wlan0 set_network 0 ssid '"SSID"'
        #wpa_cli -i wlan0 set_network 0 key_mgmt NONE
        #wpa_cli -i wlan0 set_network 0 proto WPA2
        #wpa_cli -i wlan0 set_network 0 pairwise GCMP-256 CCMP TKIP
        #wpa_cli -i wlan0 set_network 0 group GCMP-256 CCMP TKIP
        #wpa_cli -i wlan0 set_network 0 psk '"password"'
        wpa_cli -i wlan0 select_network 0
    	
        sleep 5
        media_state=$(cat /proc/mwlan/adapter0/wlan0/info  | grep -i media_state | cut -b 14-22)
        if [ "$media_state" == "Disconnec" ]; then
            let NUM_CONNECT+=1
            echo "Try to connect again.NUM_CONNECT is $NUM_CONNECT" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
        else
            echo "-----Connect success.-----" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
    	    break
        fi
    
        if [ $NUM_CONNECT -gt 4 ]; then
            echo "Stop connect NUM_CONNECT is $NUM_CONNECT" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
            break
        else
            echo "Continue connect" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
        fi

        sleep 8
    done
    
    ##DHCP
    
    #dhclient -i wlan0
    
	#Add some traffic
	iperf -c <IP> -i 1 -t 10
	
    sleep 5

    ##Stop AP
    echo "-----STOP AP-----" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
    stop hostapd

	#Add some traffic
	iperf -c <IP> -i 1 -t 10

    i=1
    while(($i<6))
    do
	mlanutl wlan0 verext 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
        echo "get verext info" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
        #echo "get verext info" 
	sleep 2
        i=$(($i+1))
    done
    
    
    dmesg > kernel_log/kernel_log.txt
 
    driver_state=$(cat /proc/mwlan/adapter0/wlan0/debug | grep -i driver_state | cut -b 14)
    echo "$driver_state" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt

    ##Dump FW log
    if [ -e /data/dump_*/file_sdio_DUMP ]; then
        echo "DUMP success" 2>&1 >> /data/test/kernel_log/script_log_$NUM.txt
        killall cycle_scan.sh
        break
    fi

    if [ $driver_state != 0 ]; then
        echo "debug_dump" > /proc/mwlan/adapter0/wlan0/config
		killall cycle_scan.sh
        break
    else
        killall cycle_scan.sh
        #rm -rf /data/test/kernel_log/kernel_log_$NUM.txt
        rm -rf /data/test/kernel_log/script_log_$NUM.txt
        let NUM+=1
    fi

    echo "Continue utimost circle."

done
```

被上面的script调用

```shell script
#!/bin/sh
#This script is used to trigger cycle scan
ERR_NUM=0
NUM=0
LOGNUM=$1

while true
do
    scan_result=$(wpa_cli -i wlan0 scan)
    if [ "$scan_result" != "OK" ]; then
		echo "scan FAIL" 2>&1 >> /data/test/kernel_log/script_log_$LOGNUM.txt
        ERR_NUM=$[$ERR_NUM + 1]
    else
        echo "scan OK" 2>&1 >> /data/test/kernel_log/script_log_$LOGNUM.txt
        ERR_NUM=0
    fi

    if [ $ERR_NUM -gt 10 ]; then
        echo "Continuos scan ERR_NUM is $ERR_NUM" 2>&1 >> /data/test/kernel_log/script_log_$LOGNUM.txt
        break
    fi

    echo "scan NUM is $NUM" 2>&1 >> /data/test/kernel_log/script_log_$LOGNUM.txt
    let NUM+=1
    sleep 8
done
```