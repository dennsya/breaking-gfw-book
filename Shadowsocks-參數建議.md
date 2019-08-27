**文獻**
* [shadowsocks-libev & acl on macOS](https://placeless.net/blog/shadowsocks-libev-&-acl-on-macos)

**建議的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

**Fast Open :**

開啟TCP Fast Open
編輯```nano /etc/sysctl.conf```，添加
```
net.ipv4.tcp_fastopen = 3
```
1 開啟客戶端，2 開啟服務端，3 都開啟

---

**Known good SIP003 plugins :**
* GoQuiet: https://github.com/cbeuw/GoQuiet
* Cloak: https://github.com/cbeuw/Cloak
* Kcptun: https://github.com/shadowsocks/kcptun
* V2ray-plugin: https://github.com/shadowsocks/v2ray-plugin