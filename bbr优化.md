# bbr 优化

## Ubuntu Server
多和一
```
wget "https://github.com/chiakge/Linux-NetSpeed/raw/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```
just bbr
```
wget -N --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && bash bbr.sh
```

## 树莓派
Raspbian 官方加入 BBR 流量拥塞控制算法。
```
sudo rpi-update
```
升级完成后重启树莓派
```
sudo reboot
```
重启之后，查看一下当前的内核：
```
uname -r


4.9.4-v7+
```


已经是 4.9.4了，现在可以启用 BBR

```
sudo bash -c 'echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf'
 
sudo bash -c 'echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf'
 
sudo sysctl -p
```
使配置生效，重启树莓派
```
sudo reboot
```
重启完成后然后可以检查一下：
```
sysctl net.ipv4.tcp_available_congestion_control

net.ipv4.tcp_available_congestion_control = bbr cubic reno
 
lsmod | grep bbr

tcp_bbr 20480 14
```
结果里边已经有 BBR 了，说明启用成功。