# iptables-samples
# 本机端口转发
iptables -t nat -A PREROUTING -p tcp --dport 22222  -j REDIRECT --to-ports 22

# 禁止访问某个IP端口，可以模拟丢包。
iptables -A OUTPUT -p tcp -d 1.2.3.4 --dport 443 -j DROP
