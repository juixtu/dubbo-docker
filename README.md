# Dubbo in Docker

修改了dubbo的版本为2.6.0,可以实现基于env的方式注册自身IP，env参数为：

```
DUBBO_IP_TO_REGISTRY — 注册到注册中心的IP地址

DUBBO_PORT_TO_REGISTRY — 注册到注册中心的端口

DUBBO_IP_TO_BIND — 监听IP地址

DUBBO_PORT_TO_BIND — 监听端口
```
