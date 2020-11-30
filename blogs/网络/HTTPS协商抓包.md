---
title: HTTPS协商抓包
date: 2020-11-29
tags:
  - https
  - 安全
categories:
 - 网络
author: Milky
publish: true
---

# 	HTTPS协商抓包

Wireshark访问百度抓包

过滤好

```
ip.dst == 183.232.231.172 or ip.src == 183.232.231.172 and tcp.port == 9297
```

![image-20201129122054079](http://static.xmilk.fun/image-20201129122054079.png)

## 分析

### TCP握手

106~108是TCP的三次握手

![image-20201129122727038](http://static.xmilk.fun/image-20201129122727038.png)

### 协商

#### 客户端Client Hello

![image-20201129133817327](http://static.xmilk.fun/image-20201129133817327.png)

查看TLS报文，暂且只关注Random和Cipher Suites 

Random字段内容是客户端生成的随机数，记作 **随机数1**

Cipher Suites是客户端支持的加密算法，可以看到里面有许多常见的加密算法字段比如AES128，AES256，这里暂不做展开



关于Cipher Suit内容参考：

Cipher Suite包含四个部分(将摘要算法并入认证算法的话也可以看作三个部分)，第一是密钥交换算法，第二是签名算法/验签算法，第三是摘要算法，第四是对称加密算法

[ssl握手协议中的CipherSuite_Netfilter,iptables/OpenVPN/TCP guard:-(-CSDN博客_ciphersuite](https://blog.csdn.net/dog250/article/details/5750992)



#### 服务端Server Hello

![image-20201129134658317](http://static.xmilk.fun/image-20201129134658317.png)

Random字段内容服务器生成的随机数，记作 **随机数2**

Cipher Suite是服务端从接收到的列表中选择的一种加密算法

这里选择的是**Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (0xc02f)**

注意这里选择的**ECDHE**其实不是加密算法，而是**密钥交换算法**

```
ECDHE（ECHD）是DH的变种
DH：（英语：Diffie–Hellman key exchange，缩写为D-H） 是一种安全协议。它可以让双方在完全没有对方任何预先信息的条件下通过不安全信道创建起一个密钥。
ECDH：（英语：Elliptic Curve Diffie–Hellman key exchange，缩写为ECDH），是一种匿名的密钥合意协议（Key-agreement protocol），这是DH的变种，采用椭圆曲线密码学来加强性能与安全性。在这个协定下，双方利用由椭圆曲线密码学建立的公钥与私钥对，在一个不安全的通道中，协商出安全的密钥。

破解难度：如果攻击者拥有大型量子计算机，那么他可以使用秀尔算法解决离散对数问题，椭圆曲线的离散对数问题破解难度比RSA破解难度低得多，但是目前还不存在建造如此大型量子计算机的科学技术，因此椭圆曲线密码学至少在未来十年（或更久）依然是安全的。

协商过程：
假设密钥交换双方为Alice、Bob，其有共享曲线参数（椭圆曲线E、阶N、基点G）。

1) Alice生成随机整数a，计算A=a*G。 #生成Alice公钥

2) Bob生成随机整数b，计算B=b*G。 #生产Bob公钥

3) Alice将A传递给Bob。A的传递可以公开，即攻击者可以获取A。

    由于椭圆曲线的离散对数问题是难题，所以攻击者不可以通过A、G计算出a。

4) Bob将B传递给Alice。同理，B的传递可以公开。

5) Bob收到Alice传递的A，计算Q =b*A  #Bob通过自己的私钥和Alice的公钥得到对称密钥Q

6) Alice收到Bob传递的B，计算Q`=a*B  #Alice通过自己的私钥和Bob的公钥得到对称密钥Q'

Alice、Bob双方即得Q=b*A=b*(a*G)=(b*a)*G=(a*b)*G=a*(b*G)=a*B=Q' (交换律和结合律)，即双方得到一致的密钥Q。
```



#### 服务端Certificate，Server Key Exchange，Server Hello Done

##### Certificate

![image-20201129141211152](http://static.xmilk.fun/image-20201129141211152.png)

服务端将自己的证书和证书的发放机构（CA）等信息告知客户端

客户端随后会对证书进行校验并从中取出**服务端公钥**

##### Server Key Exchange

![image-20201129141929342](http://static.xmilk.fun/image-20201129141929342.png)

Pubkey：基于Server Hello阶段选择的ECDHE交换密钥算法，发送它生成的椭圆曲线的公钥(经过**私钥**签名的)

Signature:签名确保Pubkey不被篡改

因为上一步已经发送了证书，客户端可以从证书中取出**公钥**，对签名验证（只能验证，无法篡改）

##### Server Hello Done

![image-20201129142209354](http://static.xmilk.fun/image-20201129142209354.png)

表示服务器需要发送的信息已经发送完毕



客户端：

如果证书校验无误，则取出公钥，对Pubkey中ECDHE算法的公钥签名校验

校验无误，通过ECDHE算法计算与客户端ECDHE算法参数结合，计算出**Pre-Master**(即ECDHE协商过程中的**Q**)

#### 客户端Client Key Exchange，Change Cipher Spec，Encrypted Handshake Message

客户端发送给服务器

这里三个包被合成一个包

![image-20201129142418579](http://static.xmilk.fun/image-20201129142418579.png)

##### Client Key Exchange

Pubkey：基于协商选择的ECDHE交换密钥算法，发送客户端生成的椭圆曲线的公钥

服务端收到后也计算出**Pre-Master（Q）**

至此，客户端/服务端都拥有 **随机数1、随机数2、Pre-Master**

通过协商的相同算法，以 **随机数1、随机数2、Pre-Master**作为参数，计算出最终密钥**Master Secret Key**用于通信

##### Change Cipher Spec

通知消息，告知对方以后的通信都使用最新的密钥加密

##### Enctypted Handshare Message

协商出密钥后，将发送一条加密的数据供服务端解密验证密钥可用



发送的加密数据：

![image-20201129163230245](http://static.xmilk.fun/image-20201129163230245.png)

#### 服务端New Session Ticket, Change Cipher Spec, Encrypted Handshake Message

##### New Session Ticket

TLS建立连接的优化，涉及缓存等，此处不分析

##### Change Cipher Spec

通知消息，告知对方以后的通信都使用最新的密钥加密

##### Encrypted Handshake Message

发送一条加密的数据供服务端解密验证密钥可用，验证成功后即可开始通信

#### 通信

![image-20201129163540457](http://static.xmilk.fun/image-20201129163540457.png)

### 流程图

![tcp三次握手](http://static.xmilk.fun/tcp三次握手.png)