---
title: 一支红杏出墙来——VPS 翻墙
categories: vps
date: 2014-12-02 03:01:49
tags:
---

VPS继续折腾中。

虽然在墙外，但是马上就回国啦，并且还可以给大家送点福利的哦。所以让我们开始这次爬墙之旅吧。

## VPS选择
当然首先是第一步你要有一个VPS，何谓之VPS呢，就是一个你摸不到但是你可以用的主机，当然这不是虚拟主机，虚拟主机对你的权限有很大的限制，但是主这个除了你听不见主机运行时的风扇声，别的你是都可以有的。

对于VPS的选择，新手一枚，也不太了解，但是之前有github大礼包，里面有100刀的digtial ocean的代金券（瞬间成土豪了），所以当然就选择了这家了，并且还挺便宜的，5刀一个月，并且还可以分时计费，如果不嫌麻烦，并且单纯是vpn的话，可以在不需要VPN的时候晚上关机，那样算下来，每月成本也就是2刀，那是大大的便宜，比买一个收费的vpn也不会贵多少，并且单人专线，岂不爽哉。

这个是给的推广链接[https://www.digitalocean.com/?refcode=5bba2d7f7d05][1]，从这注册会得到10刀，可以先试用两个月的了。

## VPN选择
对于VPN，我们也有很多选择L2TP，PPTP等，在这里我们选择更加安全的L2TP，这两个的区别可以参见

[http://www.zhihu.com/question/20174552][2]

> 通俗的说，就是如果我要用一个安全的方法，寄一个礼物给你，我有两种方法选择。一种是找一个铁盒子，把东西装在，上上锁（你有这把锁的钥匙），然后用快递发给你。这个就是PPTP；另外一种方法，我自己雇一个人，自己买一辆车，然后开车送过去。这个是L2TP。

## 设定过程
注：本文针对主机操作系统为Ubuntu 14.04的，其余操作系统在操作参数上面有些不同，开始就是因为参考的文章是给12的导致里面的配置不同，从而好久没有成功。可以参照[https://raymii.org/s/tutorials/IPSEC\_L2TP\_vpn\_with\_Ubuntu\_14.04.html][3]。
下面就开始我们的折腾之旅吧。

### 安装相应软件
```
apt-get install openswan xl2tpd ppp lsof
```
### 防火墙及内核相关设定
```
	iptables -t nat -A POSTROUTING -j SNAT --to-source 你的服务器ip -o eth+
	echo "net.ipv4.ip\_forward = 1" | tee -a /etc/sysctl.conf
	echo "net.ipv4.conf.all.accept\_redirects = 0" | tee -a /etc/sysctl.conf
	echo "net.ipv4.conf.all.send\_redirects = 0" | tee -a /etc/sysctl.conf
	echo "net.ipv4.conf.default.rp\_filter = 0" | tee -a /etc/sysctl.conf
	echo "net.ipv4.conf.default.accept\_source\_route = 0" | tee -a /etc/sysctl.conf
	echo "net.ipv4.conf.default.send\_redirects = 0" | tee -a /etc/sysctl.conf
	echo "net.ipv4.icmp\_ignore\_bogus\_error\_responses = 1" | tee -a /etc/sysctl.conf
	for vpn in /proc/sys/net/ipv4/conf/\*; do echo 0 &gt; $vpn/accept\_redirects; 
	echo 0 &gt; $vpn/send\_redirects; done
	sysctl -p
```
上面服务器ip可以用命令ifconfig查看。
直接把上面这段代码复制粘贴到terminal里面就可以了。
接下来打开/etc/rc.local文件，将下面代码替换原来里面的内容，（注：如果用vi打开文件的话可以直接用命令d+G，删除所有内容）
```
	for vpn in /proc/sys/net/ipv4/conf/\*; do echo 0 &gt; $vpn/accept\_redirects; echo 0 &gt; $vpn/send\_redirects; done
	iptables -t nat -A POSTROUTING -j SNAT --to-source 你的服务器ip -o eth+
	exit 0
```
### 配置IPSEC
编辑文件`/etc/ipsec.conf`
然后替换文件内容如下

```
version 2 # conforms to second version of ipsec.conf specification

config setup
    dumpdir=/var/run/pluto/
    #in what directory should things started by setup (notably the Pluto daemon) be allowed to dump core?

    nat_traversal=yes
    #whether to accept/offer to support NAT (NAPT, also known as “IP Masqurade”) workaround for IPsec

    virtual_private=%v4:10.0.0.0/8,%v4:192.168.0.0/16,%v4:172.16.0.0/12,%v6:fd00::/8,%v6:fe80::/10
    #contains the networks that are allowed as subnet= for the remote client. In other words, the address ranges that may live behind a NAT router through which a client connects.

    protostack=netkey
    #decide which protocol stack is going to be used.

    force_keepalive=yes
    keep_alive=60
    # Send a keep-alive packet every 60 seconds.

conn L2TP-PSK-noNAT
    authby=secret
    #shared secret. Use rsasig for certificates.

    pfs=no
    #Disable pfs

    auto=add
    #the ipsec tunnel should be started and routes created when the ipsec daemon itself starts.

    keyingtries=3
    #Only negotiate a conn. 3 times.

    ikelifetime=8h
    keylife=1h

    ike=aes256-sha1,aes128-sha1,3des-sha1
    phase2alg=aes256-sha1,aes128-sha1,3des-sha1
    # https://lists.openswan.org/pipermail/users/2014-April/022947.html
    # specifies the phase 1 encryption scheme, the hashing algorithm, and the diffie-hellman group. The modp1024 is for Diffie-Hellman 2\. Why ‘modp’ instead of dh? DH2 is a 1028 bit encryption algorithm that modulo’s a prime number, e.g. modp1028\. See RFC 5114 for details or the wiki page on diffie hellmann, if interested.

    type=transport
    #because we use l2tp as tunnel protocol

    left=104.236.130.95
    #fill in server IP above

    leftprotoport=17/1701
    right=%any
    rightprotoport=17/%any

    dpddelay=10
    # Dead Peer Dectection (RFC 3706) keepalives delay
    dpdtimeout=20
    # length of time (in seconds) we will idle without hearing either an R_U_THERE poll from our peer, or an R_U_THERE_ACK reply.
    dpdaction=clear
    # When a DPD enabled peer is declared dead, what action should be taken. clear means the eroute and SA with both be cleared.</pre>
```

编辑`/etc/ipsec.secrets`设定服务器ip以及你的秘钥如下：`服务器ip %any: PSK "秘钥"`
例如：`123.123.14.4 %any: PSK "ilovechina"`
注意这句话是放在`include /var/lib/openswan/ipsec.secrets.inc`上面的。

对于上面的秘钥，是以后你连接vpn的时候还要用的，所以呢，还是简单易记一点比较好，虽然那样就丧失安全性。这个就随意了。以上设定结束后，检查，`ipsec verify`
结果如下:
```
	Checking your system to see if IPsec got installed and started correctly:
	Version check and ipsec on-path \[OK]
	Linux Openswan U2.6.38/K3.13.0-24-generic (netkey)
	Checking for IPsec support in kernel \[OK]
	SAref kernel support \[N/A]
	NETKEY: Testing XFRM related proc values \[OK]
	\[OK]
	\[OK]
	Checking that pluto is running \[OK]
	Pluto listening for IKE on udp 500 \[OK]
	Pluto listening for NAT-T on udp 4500 \[OK]
	Checking for 'ip' command \[OK]
	Checking /bin/sh is not /bin/dash \[WARNING]
	Checking for 'iptables' command \[OK]
	Opportunistic Encryption Support \[DISABLED]
```
其中最后一个disabled，无所谓，/bin/sh的warning 也无所谓，只要其余的ok就好了。
### 配置xl2tpd
编辑文件`/etc/xl2tpd/xl2tpd.conf`
替换成下面的形式：

```
	[global]
	ipsec saref = yes
	saref refinfo = 30\`

	;debug avp = yes
	;debug network = yes
	;debug state = yes
	;debug tunnel = yes

	\[lns default]
	ip range = 172.16.1.30-172.16.1.100
	local ip = 172.16.1.1
	refuse pap = yes
	require authentication = yes
	;ppp debug = yes
	pppoptfile = /etc/ppp/options.xl2tpd
	length bit = yes
```
### 配置PPP
编辑`/etc/ppp/options.xl2tpd`
替换成下面的形式:

```
	require-mschap-v2
	ms-dns 8.8.8.8
	ms-dns 8.8.4.4
	auth
	mtu 1200
	mru 1000
	crtscts
	hide-password
	modem
	name l2tpd
	proxyarp
	lcp-echo-interval 30
	lcp-echo-failure 4
```
### 添加用户
编辑文件`/etc/ppp/chap-secrets`

```
	# Secrets for authentication using CHAP
	# client server    secret    IP addresses
	user  l2tpd    password        
```
### 重启openswan和xl2tpd

```
	/etc/init.d/ipsec restart
	/etc/init.d/xl2tpd restart
```
## 后记
如果一切正常的话，你现在就已经可以愉快的科学上网了，当然可能大多数情况下，你遇到的情况是无法连接到服务器，不过不用担心，看一下系统的log文件`/var/log/syslog 和 /var/log/auth.log`，对于error的地方，尽情的google吧。
最后就是送福利了，如果懒得配置这些还想科学上网，那就私信我吧。。。

[1]:	https://www.digitalocean.com/?refcode=5bba2d7f7d05
[2]:	http://www.zhihu.com/question/20174552 
[3]:	https://raymii.org/s/tutorials/IPSEC_L2TP_vpn_with_Ubuntu_14.04.html