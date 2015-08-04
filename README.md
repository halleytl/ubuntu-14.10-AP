##安装软件
```shell
sudo apt-get update
#开启AP基本软件
sudo apt-get install dnsmasq hostapd
#远程ssh服务软件
sudo apt-get install openssh-server
```
##添加AP配置文件, 当前AP不需要输入用户名密码
[file]/etc/hostapd/ap.conf
```shell
#ap名词test
ssid=test
#无线接口名称
interface=wlan0
hw_mode=g
driver=nl80211
#channel =1...13
channel=10
```
##修改hostapd开启脚本, 添加配置文件路径
[file]/etc/init.d/hostapd
```shell
[19]DAEMON_CONF=/etc/hostapd/ap.conf
```
##配置dns服务器
[file]/etc/dnsmasq.conf
```shell
[111]listen-address=192.168.0.1,127.0.0.1
[157]dhcp-range=192.168.0.50,192.168.0.150,12h
```
##关闭系统自带精简版的dnsmasq
[file]/etc/NetworkManager/NetworkManager.conf
```shell
[3]#dns=dnsmasq
```
##开启ap脚本
[file]~/stat.sh
```shell
#!/bin/sh
#关闭网卡
nmcli nm wifi off
rfkill unblock wlan
sleep 5
#配置dns路由地址
ifconfig wlan0 192.168.0.1
#重启hostapd
service hostapd restart
#重启dnsmasq
service dnsmasq restart
#开启转发
sysctl -w net.ipv4.ip_forward=1
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

##配置/etc/hosts文件
```shell
192.168.0.1 dashboard.helix.hichao.com
192.168.0.1 helix.hichao.com
```

