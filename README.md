# SmartDNS 配置完全指南

SmartDNS 是高性能本地 DNS 代理，支持 DoH/DoT/DoQ，自动选择最快 DNS 服务器。

## 为什么需要 SmartDNS

- 极速: 本地 DNS 缓存，秒开网站
- 安全: 支持加密 DNS (DoH/DoT/DoQ)
- 全面: 自动测速，选择最快服务器
- 灵活: 可与 Clash 配合使用

## 安装

### Linux

```bash
wget https://github.com/pymumu/smartdns/releases/download/release-36/smartdns.x86_64-linux.tar.gz
tar zxf smartdns.x86_64-linux.tar.gz
cd smartdns
./install.sh
```

### Docker

```bash
docker run -d --name smartdns \
  -p 53:53/udp -p 53:53/tcp \
  -v /opt/smartdns:/etc/smartdns \
  --restart always vimfo/smartdns
```

## 配置文件

```conf
bind 0.0.0.0:53

# 国内 DNS（直连）
server 223.5.5.5
server 119.29.29.29

# 国外 DNS（走代理）
server-tls 8.8.8.8
server-https https://dns.google/dns-query

speed-check-mode tcp:80,tcp:443
```

## 配合 Clash 使用

SmartDNS 与 Clash 的最佳配合方式：

```yaml
dns:
  enable: true
  listen: 0.0.0.0:1053
  enhanced-mode: fake-ip
  nameserver:
    - 127.0.0.1  # SmartDNS
  fallback:
    - https://doh.pub/dns-query
```

## 常见问题

**端口冲突？** 确保 53 端口未被其他服务占用。

---

推荐工具：

- [Clash for Windows](https://clashforwindows.site/)
- [ClashMI](https://clashmi.site/)
- [FlClash](https://flclash.us/)
