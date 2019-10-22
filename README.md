# gosconn

## 特性
* 断线重连: [scp协议介绍](https://github.com/ejoy/goscon/blob/master/protocol.md)
* 加密： [dh64密钥交换](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)及对称流加密
* 负载均衡
* 命名服务路由
* 配置热更新
* 支持`kcp`和`tcp`，且可以无缝切换

## 用法

断线重连服务器端代理。

```
client <--> goscon <---> server
```

`client`和`goscon`之间使用断线重连协议，`goscon`把客户端的请求内容转发到`server`。

无论`client`因为何种原因主动或被动断开连接，`goscon`都会维持对应的`server`连接，使`server`感受不到`client`断开。

在`goscon`维持连接期间，`client`可以使用断线重连协议，无缝重用之前的连接。

若`scp.reuse_time`秒没有被重用，`goscon`对断开跟`server`的连接。

编译时开启`sproto`扩展，可以新建连接后自动给后端发送一条`sproto`消息，宣布客户端的IP地址信息。

## build & run

* deps: go v1.13+

* build
```bash
go build -vendor=mod
```

* config
配置选项含义，请参考[config.go](https://github.com/ejoy/goscon/blob/master/config.go)

* run
```bash
./goscon  -alsologtostderr -v 10 -config config.yaml
```
