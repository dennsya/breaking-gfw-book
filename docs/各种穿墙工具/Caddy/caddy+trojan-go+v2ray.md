# caddy+trojan+v2ray

運作模式
* caddy 做完一個網站的80（占用），我們會在這座一個反向道另一個server網站或是自己架個網站
* trojan 負責收 443（占用），如果收到跟自己有關就處理，跟自己無關就丟給caddy 指的 80的網站
* v2ray 如果要裝上v2ray，則必須妥善處理跟trojan 的關係，因為都在443。我們需要靠trojan 分辨一下，443與trojan有關，如果跟v2ray有關，就往v2ray 丟，再沒關就往caddy 80 丟。

caddy：
* 反樣代理網站
* 自動註冊憑證
* 比nginx更加簡單易用

trojan-go：
* 測出的效能 意外比 trojan 官方庫來的快
* 集成更多方法，但是只有server 有 也沒啥用，因為客戶端都不支援

v2ray：
* 速度應當比 trojan-go 慢，但還是測試一下