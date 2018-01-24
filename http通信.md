# HTTP
* SSL和TLS的区别：TLS（transport layer security，缩写作TLS，传输层安全性协议）及其前身安全套接层是一种安全协议，目的是为互联网通信提供安全及数据完整性保障。TLS是SSL的标准。HTTPS就是HTTP ＋ SSL
![](http://liushaoqing.me/uploads/https/tls_ssl.png)
* 工作方式：客户端使用非对称加密与服务器进行通信，实现身份验证，并协商对称加密使用的密钥，然后使用协商密钥对信息进行加密通信，不同节点之间采用的对称密钥不同，从而可以保证对称密钥的安全
![](http://liushaoqing.me/uploads/https/https_ssl.svg)
* 