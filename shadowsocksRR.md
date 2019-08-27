參考文獻與**Shadowsocksr**大致相同，請參考[**shadowsocksR**](/mWSzLuMWRGyGDLnsXGgITg)

## Server
環境
```
apt-get install python git
```

安裝
```
git clone https://github.com/shadowsocksrr/shadowsocksr.git ;\
cd shadowsocksr ;\
git checkout akkariiin/dev ;\
cd shadowsocks
```

啟動

```
python server.py \
-p 遠端主機端口 \
-k 密碼 \
-m aes-256-cfb \
-o plain \
-O origin 
```

**參數配置** 
* 主要多了`auth_chain_c`,`auth_chain_d`,`auth_chain_e`,`auth_chain_f`，[***ShadowsocksRR 协议插件文档***](https://github.com/shadowsocksrr/shadowsocks-rss/blob/SSRR/ssr.md)
* [**shadowsocksR**](/mWSzLuMWRGyGDLnsXGgITg)相同

**參數選項:**

| 混淆插件 `-o`      | 協議定義插件 `-O`  |
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
環境
```
brew install python git
```

安裝
```
git clone https://github.com/shadowsocksrr/shadowsocksr.git ;\
cd shadowsocksr ;\
git checkout akkariiin/dev ;\
cd shadowsocks
```
啟動
```
python local.py \
-s 遠端主機IP \
-p 遠端主機端口 \
-k 密碼 \
-m aes-256-cfb \
-O origin \
-o plain
```