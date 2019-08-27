---
title: bbr 優化
tags: gfw
GA: UA-131051587-2
---


# bbr 優化

## Ubuntu Server
多和一
```
wget "https://github.com/chiakge/Linux-NetSpeed/raw/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```
just bbr
```
wget -N --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && bash bbr.sh
```

## 樹莓派
Raspbian 官方加入 BBR 流量擁塞控制算法。
```
sudo rpi-update
```
升級完成後重啟樹莓派
```
sudo reboot
```
重啟之後，查看一下當前的內核：
```
uname -r


4.9.4-v7+
```


已經是 4.9.4了，現在可以啟用 BBR

```
sudo bash -c 'echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf'
 
sudo bash -c 'echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf'
 
sudo sysctl -p
```
使配置生效，重啟樹莓派
```
sudo reboot
```
重啟完成後然後可以檢查一下：
```
sysctl net.ipv4.tcp_available_congestion_control

net.ipv4.tcp_available_congestion_control = bbr cubic reno
 
lsmod | grep bbr

tcp_bbr 20480 14
```
結果裡邊已經有 BBR 了，說明啟用成功。