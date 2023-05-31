# shadowsocksR

[***shadowsocksr-backup/shadowsocksr***](https://github.com/shadowsocksr-backup/shadowsocksr/tree/manyuser/shadowsocks)
**参数建议文献**
* [**ssr和ss链接解码(decode)**](https://www.kingr.top/2018/10/07/ssr-decode/)
* [ShadowsocksR 配置](https://www.zfl9.com/ssr.html)
* [***ShadowSocksR(SSR)功能详细介绍及使用教程***](https://www.quchao.net/ShadowsocksR.html?fbclid=IwAR22Zxg1u4Zkka82c6VxtmNuTlsAdINJYXnxDgItuyqM5tjZ6d0Yz-5KF14) 超级无敌详细
* [[ShadowsocksR] 大概是萌新也看得懂的SSR功能详细介绍&使用教程](https://moe.best/tutorial/shadowsocksr.html?fbclid=IwAR0yVZmo6Xx3UWYdQcHCHgRh_M8YWjfX0p7DKcuAKkzVQ6gxUlXsHQJebU4)

## Server
环境
```
apt-get install python git
```

安装
```
git clone https://github.com/shadowsocksrr/shadowsocksr.git ;\
cd shadowsocksr ;\
git checkout akkariiin/dev ;\
cd shadowsocks
```

启动

```
python server.py \
-p 远端主机端口 \
-k 密码 \
-m aes-256-cfb \
-o plain \
-O origin 
```

**参数配置** 
* ``-o``： ``http_simple``, ~~``tls1.2_ticket_auth``~~，``-p``设置为 80、~~443~~。
    * 进阶效果更好：[**SSR混淆及混淆协议参数的设置**](https://sobaigu.com/how-to-use-ssr-obfs.html)
* `-O` ： `chain_a`, `chain_b`, `auth_aes128_md5`或`auth_aes128_sha1`
    * 选`chain_a`, `chain_b`的话，`-m`就选`none`，因为本身自带`RC4`

[***ShadowsocksR 协议插件文档***](https://github.com/shadowsocksr-backup/shadowsocks-rss/blob/master/ssr.md)


**参数选项:**

| 混淆插件 `-o`          | 协议定义插件 `-O`  |
|------------------------|--------------------|
| ~~plain~~              | ~~origin~~         |
| http_simple            | ~~verify_deflate~~ |
| http_post              | ~~verify_sha1~~    |
| ~~random_head~~        | ~~auth_sha1~~      |
| **tls1.2_ticket_auth** | ~~auth_sha1_v2~~   |
|                        | ~~auth_sha1_v4~~   |
|                        | auth_aes128_md5    |
|                        | auth_aes128_sha1   |
|                        | **auth_chain_a**   |
|                        | **auth_chain_b**   |

---

## Client
环境
```
brew install python git
```

安装
```
git clone https://github.com/shadowsocksrr/shadowsocksr.git ;\
cd shadowsocksr ;\
git checkout akkariiin/dev ;\
cd shadowsocks
```
启动
```
python local.py \
-s 远端主机IP \
-p 远端主机端口 \
-k 密码 \
-m aes-256-cfb \
-O origin \
-o plain
```