# 代码生成

### 1 基本概念
先看下图，`Thrift` 网络架构。  
![thrift network stack](../images/thrift-network-stack.jpg)

#### 1.1 Transport （传输层）

传输层提供了一些简单抽象的从网络中读写的接口，这使 `Thrift` 能够将底层传输与系统的其余部分分离（例如，序列化/反序列化）。

传输层暴露的接口有：

* open
* close
* read
* write
* flush

除了上面的 `Transport` 接口之外，`Thrift` 还使用 `ServerTransport` 接口来接收或创建原始传输对象，也就是说，`ServerTransport` 主要用于服务器端，为传入连接创建新的传输对象。`ServerTransport` 接口有：

* open
* listen
* accept
* close

















































