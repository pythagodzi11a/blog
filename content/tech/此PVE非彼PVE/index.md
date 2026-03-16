---
title: "此PVE非彼PVE"
date: 2026-02-12T20:00:00+08:00
draft: true
tags:
  - 运维
  - PVE
  - SSH
  - 网络安全
  - 吐槽
---

## 无意义

最近在抖抖看到过一句话叫做，几万年前人类的科技树就已经PVE霸服了，剩下的技术都是PVP卷出来的。

我确实是非常的感同身受。今天我的gitea没连上，想要去ssh到服务器上看看，才发现穿透出来的ssh端口也在被爆破，导致我的正常连接会因为MaxStartups被断联。真是傻逼的没边了。

说实在话如果舍弃掉所有的安全措施，我的服务器在刚开始是非常之稳定非常之好用的，但是由于持续的攻击导致我必须加入各种防护措施，性能白白的被浪费了，全是为了防范这些攻击。你要说他们没错吧，攻击跟疯狗一样，你说他们错了吧，又并非不能理解。实在是让人感到矛盾。不过，抛开理性的逻辑，这就是群傻逼。

人类就是喜欢在向内的螺旋里陷入癫狂，但凡攻击我的服务器是为了促进信息共享，是为了互联网精神，是为了互联网更伟大的进化，是为了人与人之间的互联，是为了反抗日渐原子化的趋势，那我毫无怨言。

## 措施

所以现在只好这样了，继上次之后，又得新加入几个措施。

### knockd

虽然我的中转服务器已经设置了fail2ban，但是我暴露出来的ssh端口仍然有特征会被爆破，所以现在我需要加入knockd，这很麻烦，真的很麻烦。
这边是knockd的默认配置，我保存一下

```toml(filename="/etc/knockd.conf")
[options]
        UseSyslog

[openSSH]
        sequence    = 7000,8000,9000
        seq_timeout = 5
        command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 8891 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9000,8000,7000
        seq_timeout = 5
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 8891 -j ACCEPT
        tcpflags    = syn

[openHTTPS]
        sequence    = 12345,54321,24680,13579
        seq_timeout = 5
        command     = /usr/local/sbin/knock_add -i -c INPUT -p tcp -d 443 -f %IP%
        tcpflags    = syn

```

### 更改frps设置

经由攻击，我发现以前我的frp能用即可的配置过于简陋了，所以我必须更加改进我的frps配置来达到更modified更合适的状态

### 更改网络结构

以前暴露了过多的端口导致攻击频繁，frp建立的连接太多，现在打算中转服务器只设置SNI Proxy，服务端口只在内网，ssl卸载也可以部署在内网，这样好处是暴露端口减少，低级的扫描无法得知我的各种策略，难以直接进行攻击。
