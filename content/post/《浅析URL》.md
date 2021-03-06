---
title: "《浅析URL》"
date: 2019-09-28T22:13:25+08:00
draft: false
---

# IP(Internet Protocal)

- IP 协议约定了两件事

  1. 如何定位一台设备
  2. 如何封装数据报文，以跟其他设备交流。

- IP 又分内网 IP 和外网 IP
  内网和外网是依靠路由器链接的，从内网到外网需要经过路由器，从外网到内网还是需要路由器，所以路由器又被称为网关。

* 几个特殊的 IP
  1. 127.0.0.1 表示自己
  2. localhost 是通过 hosts 文件指定为自己，可以人为修改 hosts 文件比如修改为 127.0.0.1 carminepink 那么我们就可以用 cariminepink 代替 localhost 了。
  3. 0.0.0.0 不代表任何设备

# 端口(port)

一台机器可以提供很多服务，每个服务就需要一个端口才能请求到。就好比你去饭厅点餐，在主食窗口提供的只有主食，饮料窗口提供的只有饮料，你不能在主食窗口点饮料，反过来也一样。

- 下面几个端口需要特殊记忆。

  1. HTTP 对应 80 端口
  2. HTTPS 对应 443 端口
  3. FTP 对应 21 端口

- 一台机器一共有 65535 个端口。
- 0-1023 号端口是提供给系统使用的，用户不能使用。
- http-server 默认使用 8080 端口
- 一个端口被占用的话用户必须换另一个端口
- IP 和端口缺一不可
  因为 IP 定位一个设备，端口定位这个设备所提供的服务。

# 域名/域名系统 DNS(Domain Name System)

首先我们得知道域名是对 IP 的一种别称，因为用户总不会每次访问网站要记住类似于 183.162.144.79 这样的 IP 地址吧，所以就有了域名方便记忆。例如 baidu.com 就是一个域名，它所对应的 IP 有两个，分别是 183.232.231.174 和 39.156.69.79

- 一个域名可以对应多个 IP，如上，要想知道某个域名对应的 IP 可以在终端输入 ping 对应的域名 即可知道。
  比如 ping baidu.com 就可以知道它对应的 IP 有 183.232.231.174 和 39.156.69.79

* 一个域名对应多个 IP 的原因是防止用户访问数过大时服务器崩溃。
* 一个 IP 可以对应多个域名，这样只能说明你们公司很穷，要和别人合作公用一个域名。

## DNS 的作用

DNS 的作用就是把互联网上的主机名字转换为对应的 IP 地址。

- 举例:我在网址栏输入域名 baidu.com 之后发生的整个过程
  1. 我的浏览器会向运营商的 DNS 服务器询问 baidu.com 对应的 IP
  2. 运营商的 DNS 服务器把正确的 IP 反馈给浏览器
  3. 浏览器再向该域名对应的 IP 的 80/443 端口发送请求
  4. 请求的内容就是展示 baidu.com 网页的内容

## 域名的结构

- 以 www.cctv.com 为例
  com 是顶级域名，cctv 是二级域名，www 是三级域名。

- DNS 规定，级别最低的域名写在最左边，级别最高的顶级域名写在最右边。

所以 www.baidu.com 和 baidu.com 不是同一个域名，前者是后者的子域名。

# URl(Uniform Resource Locator)

- URL 的组成
  `协议+域名或IP+端口号+路径+查询字符串+锚点`

## URL 举例

https://www.baidu.com/s?wd=hello&rsv_spt=1#5
![上面的网址解析](/images/url.jpg)
需要注意的是

- 锚点虽然看起来可以是中文，但是实际上是不支持中文的，因为机器会自动把中文转换成机器能够识别的语言。
- 锚点(#~)是不能在开发者工具 network 面板中查看到的，因为锚点不会传给服务器。

## 跨域

所谓的跨域，举例说明：

- 如果 A 页面和 B 页面的协议、域名、端口号、子域名有一个不一样，那么两个网页所进行的访问行动都是跨域的，而浏览器为了安全问题起见，一般都对跨域访问进行了限制，不允许跨域访问其他页面的资源。
  注意！！！

* 其实跨域限制访问，是浏览器的限制，就是因为浏览器的同源策略造成的。

### 同源策略

是指协议、域名、端口号都要相同，有一个不相同就会产生跨域。
