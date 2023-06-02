apt-get update

apt-get install -y haproxy
---

sudo nano /etc/haproxy/haproxy.cfg

global
user haproxy
group haproxy

defaults
mode http
log global
retries 2
timeout connect 3000ms
timeout server 5000ms
timeout client 5000ms

frontend kubernetes
bind 192.168.15.198:6443
option tcplog
mode tcp
default_backend kubernetes

backend kubernetes
mode tcp
balance roundrobin
option tcp-check
server master 192.168.15.195:6443 check fall 3 rise 2
---

sudo systemctl restart haproxy
sudo systemctl status haproxy