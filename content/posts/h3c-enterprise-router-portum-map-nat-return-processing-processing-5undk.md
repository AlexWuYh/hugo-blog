---
title: H3C企业路由器端口映射NAT回流问题处理
slug: h3c-enterprise-router-portum-map-nat-return-processing-processing-5undk
date: '2024-07-27 22:24:41+08:00'
lastmod: '2024-07-27 22:27:11+08:00'
toc: true
isCJKLanguage: true
---

# H3C企业路由器端口映射NAT回流问题处理

最近公司搬到新办公室了，换了H3C的MSR系列企业路由器，通过WEB页面配置好`WAN口`​以及`nat端口映射`​后，从外部公网可以正常访问该端口映射，但是从`内网主机`​访问`WAN口公网IP的端口`​时却不能正常访问。百思不得其解...后面通过抓包和查阅资料才发现是因为**NAT回流**导致的。

### NAT回流的原因是：

* ​`内网主机A`​访问`WAN口IP和端口`​，通过端口映射去请求`内网服务器B`​时，会先把流量送到`网关设备的WAN口`​
* 网关设备的WAN口会配置`NAT SERVER`​，这个时候它收到该数据包时通过查询NAT端口映射表，就会修改报文中的`目的地址`​和`目的端口`​(**内网服务器B的IP和端口**), 但是**不会对源地址进行修改**。

  * 之所以不修改源地址，是因为从外部公网访问时，需要知道**返程IP**，访问端才能正常收到从网关设备返回的报文。
* 这时报文被发送到`内网服务器B`​ **，并且源地址IP是**​`内网主机A`​
* 因为`源地址`​没有修改，还是`内网主机A`​的IP，这时`内网服务器B`​回复数据报文时，目的地址和源地址对调，数据报文就会直接通过内网交换机转发给`内网主机A`​
* ​`内网主机A`​收到回复报文，就会发现报文中的`源地址IP`​与自己请求出去时的`目的地址IP`​不一致，就会丢弃报文，最终就访问失败

### 怎么解决NAT回流的问题：

通过查阅资料发现H3C V7版本的路由器支持`NAT hairpin`​功能，在网关设备的内网侧接口上启用`NAT hairpin`​ **，** 就可以实现内网主机通过公网地址和端口来访问内网的其他服务器或主机了。

具体配置如下：

```plain
#
interface GigabitEthernet0/1                                                                                                                                                                        
  port link-mode route                                                                                                                                                                               
  combo enable fiber                                                                                                                                                                                 
  ip address 10.10.10.1 255.255.255.0                                                                                                                                                                
  packet-filter name WebHttpHttps3 inbound                                                                                                                                                           
  nat hairpin enable                                                                                                                                                                                 
  undo dhcp select server
#
```

​![](https://imgup.oneone.life/app/hide.php?key=dHpPaUpLTUJRZDBmbFJ1QzJLRThQRFFwS1dwamlFRTZSc25L)​

配置完之后，再抓包就会发现，从`网关设备`​发送给`内网服务器B`​的数据包中的`源地址IP`​已经被替换成`WAN口IP地址`​

* 因为隐私的问题，定位和配置回流后抓包截图没有放上来。大家感兴趣可以自行抓包查看。

> 这里就忍不住要吐槽一下H3C这个Web界面做的真差，在配置时**有一些配置并不会写到后台配置中**，还需要去后台命令行再手动配置。之前配置VPN的时候就也遇到类似的问题

‍
