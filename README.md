# Dubbo in Docker

修改了dubbo的版本为2.6.0,可以实现基于env的方式注册自身IP，env参数为：

```
DUBBO_IP_TO_REGISTRY — 注册到注册中心的IP地址

DUBBO_PORT_TO_REGISTRY — 注册到注册中心的端口

DUBBO_IP_TO_BIND — 监听IP地址

DUBBO_PORT_TO_BIND — 监听端口
```
# 在rancher中的部署方式

镜像已经制作完成：

```
#client端镜像
juixtu/service-consumer:v1

#server端镜像
juixtu/service-producer:v1
```
## 部署zookeeper

为了模拟环境情况，zk通过docker的方式部署到集群外：
```
docker run -d -p 2181:2181 registry.aliyuncs.com/acs-sample/zookeeper:3.4.8
```

# 部署client

正常通过rancher UI部署client服务，需要注意的暴露方式选择nodeport 端口为8899，环境变量填写自身的注册地址、zk地址和暴露的端口：
```
ZOOKEEPER_SERVER=[zk地址]
DUBBO_IP_TO_REGISTRY=[需要注册的地址] - 通过field.status.HostIP转换
SERVER_PORT=8899
```
#部署server

正常通过rancher UI部署server服务，需要注意的暴露方式选择nodeport 端口为20880，环境变量填写自身的注册地址、zk地址：

```
ZOOKEEPER_SERVER=[zk地址]
DUBBO_IP_TO_REGISTRY=[需要注册的地址] - 通过field.status.HostIP转换
```
## 测试
正常通过client服务的nodeport地址就可以访问，并返回相应的dubbo docker提示，在server服务所在的节点使用[ conntrack —L | grep 20080 ]可以查看到相应的的长链接已经建立
