# xrayExample
caddy反代xray(v2ray)的示例，根据协议和path反代到不同的xray端口。使用回落的话，服务器指纹似乎仍然是xray的，所以尽量用反代。

各种协议包括ws，h2，grpc

server.json里面不能有TLS设置，就是client是trojan+各种协议+tls，server是trojan+各种协议，把tls部分全部交于caddy处理，解密后才反代到server。

caddy使用了泛域名，xray的服务器配置尽量不配置域名，所以可以基本不修改就应用到不同的服务器，只需要client指定不同的IP和域名就可以。

0.因为client主要都是使用客户端可视化配置，所以没放客户端配置。参考服务器配置，加上TLS就好，注意path等设置。

1.考虑到v2ray5那边说vless被放弃，所以即使当前用的是xray，仍然尽量使用trojan而没用vless。

2.v2ray5使用trojan+ws遇到莫名其妙的问题(没仔细测试)，只能是vless+ws，大概应该是程序BUG，也可能是我手潮。

3.h2的服务器设置里必须填上host，不填的话，也会有问题，但可以填多个，所以可以把几个VPS的HOST都填上。

4.h2似乎无法过CDN，最初有说明说CloudFlare不支持http/2回源，但是看最新文档似乎是支持了，但还是不行。不知道原因。另两个支持CDN。