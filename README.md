# iptables-samples

## kernel参数修改， 允许IP转发。
### 长期修改
修改/etc/sysctl.conf  
net.ipv4.ip_forward=1  
执行sysctl -p加载
### 临时修改
echo 1 > /proc/sys/net/ipv4/ip_forward

## 本机端口转发
iptables -t nat -A PREROUTING -p tcp --dport 22222  -j REDIRECT --to-ports 22

## 转发收到的消息到其他IP（端口）
iptables -t nat -A PREROUTING -p tcp -d 1.1.1.1 -j DNAT --to 2.2.2.2
iptables -t nat -A PREROUTING -p tcp -d 1.1.1.1 --dport 1111 -j DNAT --to 2.2.2.2:2222

## 转发发出的消息到其他IP（端口）
iptables -t nat -A OUTPUT -p tcp -d 1.1.1.1 -j DNAT --to 2.2.2.2
iptables -t nat -A OUTPUT -p tcp -d 1.1.1.1 --dport 1111 -j DNAT --to 2.2.2.2:2222

## 禁止访问某个IP端口，可以模拟丢包。
iptables -A OUTPUT -p tcp -d 1.2.3.4 --dport 443 -j DROP

## 源地址转换
iptables-t nat -A POSTROUTING -s 10.8.0.0/255.255.255.0 -o eth0 -j SNAT --to-source 192.168.5.3
iptables-t nat -A POSTROUTING -s 10.8.0.0/255.255.255.0 -o eth0 -j SNAT --to-source192.168.5.3-192.168.5.5

## 地址伪装，与SNAT的区别是，自动从网卡获取地址
iptables-t nat -A POSTROUTING -s 10.8.0.0/255.255.255.0 -o eth0 -j MASQUERADE
