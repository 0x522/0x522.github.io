# URL


## URL 包含哪几部分

URL 包括:

- 协议
- 域名/ip 地址
- 路径
- 查询参数
- 锚点<br>
  **以`https://www.baidu.com/s?wd=hello&rsv_spt=1#5` 为例**<br>
  https 代表超文本传输协议<br>
  www.baidu.com 代表域名<br>
  s?wd=hello&rsv_spt=1 代表查询参数<br>
  #5 代表锚点，可以用来网页定位<br>

## DNS

域名系统（英语：Domain Name System，缩写：DNS）是互联网的一项服务。它作为将域名和 IP 地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。DNS 使用 TCP 和 UDP 端口 53。当前，对于每一级域名长度的限制是 63 个字符，域名总长度则不能超过 253 个字符。

## nslookup

nslookup 是一种网络管理 命令行工具，可在许多计算机操作系统中使用，用于查询域名系统（DNS）以获得域名或 IP 地址映射或其他 DNS 记录。名称“ nslookup ”表示“ 名称服务器查找 ”。<br>
简单来说，nslookup 用来查找域名对应的 ip 地址。许多大企业的域名对应多个 ip 地址，可以实现不同地区的流量分流，叫做负载均衡。

## IP

**引言**<br>
互联网协议套件(英语：Internet Protocol Suite，缩写 IPS)是一个网络通信模型，以及一整个网络传输协议家族，为网际网络的基础通信架构。它常被通称为 TCP/IP 协议族（英语：TCP/IP Protocol Suite，或 TCP/IP Protocols），简称 TCP/IP。因为该协议家族的两个核心协议：TCP（传输控制协议）和 IP（网际协议），为该家族中最早通过的标准。由于在网络通讯协议普遍采用分层的结构，当多个层次的协议共同工作时，类似计算机科学中的堆栈，因此又被称为 TCP/IP 协议栈(英语：TCP/IP Protocol Stack)。<br>
**IP 协议就是 网际协议（英语：Internet Protocol，缩写：IP；也称互联网协议）是用于分组交换数据网络的一种协议。**<br>

## ping 命令

**引言**<br>
ICMP 协议是“Internet Control Message Ptotocol”（因特网控制消息协议）的缩写。它是 TCP/IP 协议族的一个子协议，用于在 IP 主机、路由器之间传递控制消息。<br>
ping (Packet Internet Groper)，因特网包探索器，用于测试网络连接量的程序。Ping 发送一个 ICMP；回声请求消息给目的地并报告是否收到所希望的 ICMP echo （ICMP 回声应答）。它是用来检查网络是否通畅或者网络连接速度的命令。<br>
**ping 命令通常用来作为网络可用性的检查。ping 命令可以对一个网络地址发送测试数据包，看该网络地址是否有响应并统计响应时间，以此测试网络。**
**ping 和 ICMP 的关系：ping 命令发送数据使用的是 ICMP 协议。**<br>

**ping 的原理：**
向指定的网络地址发送一定长度的数据包，按照约定，若指定网络地址存在的话，会返回同样大小的数据包，当然，若在特定时间内没有返回，就是“超时”，会被认为指定的网络地址不存在。

ICMP 协议通过 IP 协议发送的，IP 协议是一种无连接的，不可靠的数据包协议。在 Unix/Linux,序号从 0 开始计数，依次递增。而 Windows ping 程序的 ICMP 序列号是没有规律。

## 域名是什么

网域名称（英语：Domain Name，简称：Domain），简称域名、网域，是由一串用点分隔的字符组成的互联网上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位。域名可以说是一个 IP 地址的代称，目的是为了便于记忆后者。<br>

### 分别有哪几类域名

**顶级域名（TLD）**

域名由两个或两个以上的词构成，中间由点号分隔开，最右边的那个词称为顶级域名 (TLD, top-level domain)，如 .com .cn 等。

顶级域名又分为三类：一是国家和地区顶级域名（country code top-level domain，简称 ccTLD），目前 200 多个国家都按照 ISO3166 国家代码分配了顶级域名，例如中国是 cn，日本是 jp 等。二是通用顶级域名（generictop-level domain，简称 gTLD），例如表示工商企业的 .com，表示网络提供商的 .net，表示非盈利组织的 .org 等。三是新顶级域（New gTLD）如.xinss

- 通用顶级域名（gTLD）

  通用顶级域名英文全称为 Generic top-level domain，简称为 gTLD。通用顶级域名顾名思义其没有国家和地域限制，为互联网数字分配机构 IANA（Internet Assigned Numbers Authority）管理维护。通用顶级域名又按是否为赞助又可分为两类，一类是非赞助顶级域名（unsponsored top-level domains），其为人们所常见，使用范围较广，包括 .com .net .org .biz s.info .name .pro。另一类为赞助顶级域名（sponsored top-level domains(sTLD)），其包含 .gov .edu .mil .mobi .tel .cat 等。

- 国家和地区顶级域名（ccTLD）

  国别顶级域名英文全称为 Country code top-level domain，简称为 ccTLD，为某个国家或地区所专属域名后缀，基本均为国家代码，所以我们只要看到某个域名后缀为两字符，都是国别域名，另外部分国别顶级域名必需提供该国的一些证明材料方可进行注册。比如我们较为常见的 .cn(中国) .co(哥伦比亚共和国) .cc（Cocos 岛国） .hk（香港） .de（德国） .ru（俄罗斯） .us（美国）等，但有些国别域名后缀因其具有输入快捷、具有特定含义或行业属性等优势，也逐渐变得较为通用起来，比如有 .tv(电视、直播) .co(公司、商业)，还有最近比较流行的 .vc(风投)等。

- 新顶级域名（New gTLD）

  新顶级域名全称 New Generic Top-level Domain，简称 new gTLD 或 ngTLD。其实新顶级域名后缀本属于通用顶级域名（gTLD），只是因为其推出时间较晚（2014 年），数量众多、影响非常深远，故将其单独划分为一类。
  由于在 .com 精品资源日益枯竭的情况下，ICANN 为满足广大初创企业、机构团体和个人等对优质域名的迫切需求，2011 年 6 月 20 日 ICANN 于新加坡会议上正式通过新顶级域(New gTLD)批案，任何公司、机构都有权向 ICANN 申请新的顶级域名。新顶级域名的推出，切实有效地降低了初创公司及新推品牌的域名成本，同时优化了传统的搜索方式，另外也有利于国际化大公司进一步巩固和强化自身品牌，从而真正地激发了全球经济活力。<br>

  **新顶级域名按功能属性，大致可划分为通用型、行业型、地域型、公司品牌型四大类：**

  1. 通用型的域名后缀为没有很强的行业属性和限定含义的域名后缀，比较有代表性的有：.club .xyz .xin .top .web .app .wang 等。
  2. 行业型有：.game .car .bank .club .news .shop .store .cloud .music .fund .pub .bid .pet .tour .men .商标等，为新顶级域名主力军，数量众多，基本上涵盖各行各业。
  3. 地域型主要以城市后缀为主，有：.nyc .berlin .paris .london 等。
  4. 公司品牌型主要为世界五百强企业申请，有：.alibaba .taobao .baidu .google .apple .sky 等。

**国际化域名（IDN）**

域名按语言种类可划分为两大类。一类是英文域名，由 26 个英文字母、数字和中划线（-）构成；另一种即为国际化域名，即 IDN （InternationalizedDomain Names）也称多语种域名，是指非英语国家为推广本国语言的域名系统的一个总称。含有中文的域名为中文域名，比如中文顶级域名有.中国、.商店、.广东、.世界等，含有日文的为日文域名，如日文域名コム .com，含有阿拉伯文的为阿拉伯域名，含有韩文的为韩文域名等等。顶级域名、二级域名、三级域名等均可以为 IDN。

