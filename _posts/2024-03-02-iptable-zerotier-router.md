---
layout: post
title: "zerotier路由"
date: 2024-03-02 14:14:00
---

sysctl -w net.ipv4.ip_forward=1

iptables -F
iptables -t nat -A POSTROUTING -o eth0 -s 10.10.1.0/24 -j SNAT --to-source 175.202.39.440
iptables -t filter -A FORWARD -i zt+ -s 10.10.1.0/24 -d 0.0.0.0/0 -j ACCEPT
iptables -t filter -A FORWARD -i eth0 -s 0.0.0.0/0 -d 10.10.1.0/24 -j ACCEPT
apt install iptables-persistent
//以下是ipv6
ip6tables -t nat -A POSTROUTING -o ztiv5dfz7r -j MASQUERADE
