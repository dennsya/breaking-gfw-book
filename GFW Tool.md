---
title: GFW Tool
tags: gfw
image: https://i.imgur.com/PCGNueO.jpg
GA: UA-131051587-2
---
[**主頁**](https://hackmd.io/@xrp4k0iHSfeGBDMiQ8kkzQ/SkaWsunMB/%2FuOfRBTx0SAq7xMx426pIUg)
# GFW Tool：
**作者對翻牆的定義：未必要用proxy,vpn，只要能夠耍手段達到自己要的目的都是翻牆。如抵抗DNS污染的[hosts項目](https://github.com/googlehosts/hosts)（雖然已失能）**

shadowsocks,shadowsocksr,shadowsocksrr算是同一個家族。
## [Shadowsocks](/vwZIuQSCThiWDxX31-rzjQ)
貢獻：最早提出**socks5的加密代理概念**，程式碼質量最好。
> 
**架構：**<server>-<ss-server>-加密socks5流量-<ss-client>-<client>
* 優點：代碼質量好、效率高、高可擴展性，要擴展才會好用。
* 缺點：擴展的插件設定較為複雜。
* 現況：中國防火牆已經可以完整識別。[相關論文清單](https://github.com/shadowsocks/papers/blob/master/README.md)

> ### [simple-obfs](/hgX2urnXQ2SYs0iyGLGQ3Q)
> 貢獻：將封包偽裝成http,tls
> 
> **架構：** <server>-<ss-server>-<simple-obfs>-**偽裝成http/tls封包流量**-<simple-obfs>-<ss-client>-<client>
> * 優點：快速搭建、簡單設定、多客戶端支援、執行效率高。
> * 缺點：已被放棄維護。
> * 現況：已經被放棄，因為只有封包偽裝，沒有tls握手動作（會被中國防火牆識別沒有握手行為），但還是能用。

> ### [kcptun](/htgJdSbuSVytx2yiPgylMg)
> 貢獻：基於KCP 協議的UDP 隧道，它可以將TCP 流轉換為KCP+UDP 流。而KCP 是一個快速可靠協議，能以比TCP 浪費10%-20%的帶寬的代價，換取平均延遲降低30%-40%，且最大延遲降低三倍的傳輸效果。
> 
> **架構：** <server>-<ss-server>-<kcptun-server>-**封包流改成UDP**-<kcptun-client>-<ss-client>-<client>
> * 優點：速度真的快。
> * 缺點：要搭建兩個server端、兩個client端有點麻煩。
> * 現況：多家雲服務供應商可能會很討厭UDP流量，所以主機有被VPS封鎖的風險。

> ### [v2ray-plugin](/zT220UA0Sgy9xZV0eYcYuA)
> 貢獻：繼承、改進simple-obfs只有偽裝的部分，他實現了tls握手。可是設定真的複雜。
>
> **架構：** <server>-<ss-server>-<v2ray-plugin>-**模擬tls握手行為＋偽裝成tls封包流量**-<v2ray-plugin>-<ss-client>-<client>
> * 優點：可以模擬tls握手騙過中國防火牆。
> * 缺點：搭建過程繁瑣，要憑證、cloudflar，客戶端支持少。
> * 現況：速度雖然快不起來，但是超穩。

## [shadowsocksR](/mWSzLuMWRGyGDLnsXGgITg), [shadowsocksRR](/9LcDTEH2SHSvsoghwpHtkg)
貢獻：在shadowsocks還沒有這麼多plugin時，shadowsocksR的作者就已經嘗試http,random head各式各樣的風包偽裝。當中tls的偽裝方法跟tor project中的[meek plugin](https://trac.torproject.org/projects/tor/wiki/doc/meek) 相當相似。
* 優點：設定方便、擴展插件選項多。
* 缺點：幾乎都是python撰寫，效率低，CPU負荷大、錯誤多，而且停止開發了。

## v2ray
最新的專案
* 優點：方法多、效果好
* 缺點：設定複雜、客戶端不完整

**擁塞控制優化**
穿牆是負責欺騙，但是欺騙不夠，可以是因為海底線纜物理現象太糟糕，導致極度不穩定。所以需要委託kernel的封包控制管理。

**通信軟體Proxy**
目前只見過telegram
> MTProxy 這是telegram 自己專屬設計的代理協議，設計架構感覺很複雜應該很安全。