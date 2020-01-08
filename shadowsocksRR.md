# shadowsocksRR

参考文献与**Shadowsocksr**大致相同，请参考[**shadowsocksR**](/mWSzLuMWRGyGDLnsXGgITg)

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
* 主要多了`auth_chain_c`,`auth_chain_d`,`auth_chain_e`,`auth_chain_f`，[***ShadowsocksRR 协议插件文档***](https://github.com/shadowsocksrr/shadowsocks-rss/blob/SSRR/ssr.md)
* [**shadowsocksR**](/mWSzLuMWRGyGDLnsXGgITg)相同

**参数选项:**

| 混淆插件 `-o`      | 协议定义插件 `-O`  |
|--------------------|--------------------|
| ~~plain~~          | ~~origin~~         |
| http_simple        | ~~verify_deflate~~ |
| http_post          | ~~verify_sha1~~    |
| ~~random_head~~    | ~~auth_sha1~~      |
| tls1.2_ticket_auth | ~~auth_sha1_v2~~   |
|                    | ~~auth_sha1_v4~~   |
|                    | auth_aes128_md5    |
|                    | auth_aes128_sha1   |
|                    | auth_chain_a       |
|                    | auth_chain_b       |
|                    | **auth_chain_c**   |
|                    | **auth_chain_d**   |
|                    | auth_chain_e       |
|                    | auth_chain_f       |

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