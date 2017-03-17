Deploying the IPTL orchestrator
===

The global IPTL orchestrator needs a few moving parts to work around the lack of public IPv4 address space within the community that runs the lablets. Unfortunately, despite my best efforts, Kubernetes is not ready to handle node-to-node communication over IPv6 as of March 2017, but we're preparing for the day that it can.

Installing cjdns (optional but recommended)
---
1. Follow the instructions from the [cjdns install guide](https://github.com/cjdelisle/cjdns#how-to-install-cjdns) and create a UDPInterface gateway on your master node so that client nodes can connect.

Installing L2TP server (required for operation behind NAT)
---
(This guide suggests Ubuntu 16.04, as of March 2017 it has the best support for the Kubernetes prereqs.)

1. Install xl2tpd: ` apt-get install xl2tpd`
1. Enable ip_forwarding in sysctl `sudo bash -c "echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf && sysctl -p /etc/sysctl.conf"`
1. Edit `/etc/xl2tpd/xl2tpd.conf` using the following **as root**:
```
cat <<EOF > /etc/xl2tpd.conf
[global]
port = 1701
access control = no

[lns default]
ip range = 172.21.0.2-172.21.255.254
local ip = 172.21.0.1
require authentication = yes
name = K8SVPN
pppoptfile = /etc/ppp/options.xl2tpd
EOF
```
4. Edit `/etc/ppp/options.xl2tpd` using the following **as root**:
```
cat <<EOF > /etc/ppp/options.xl2tpd
noccp
auth
crtscts
mtu 1410
mru 1410
nodefaultroute
lock
proxyarp
silent
EOF
```
5. Edit `/etc/ppp/chap-secrets` using the following **as root**:
```
cat <<EOF > /etc/ppp/chap-secrets
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
"protocol_labs" *       "protocol_labs_k8s"     *
EOF
```
6. Restart `xl2tpd` using one of the following depending on your init system:
```
sudo systemctl restart xl2tpd
### OR ###
sudo service xl2tpd restart
```

Installing Kubernetes
---


1. Install the setup framework using the directions from the following page: [Installing Kubernetes on Linux with kubeadm](https://kubernetes.io/docs/getting-started-guides/kubeadm/)
1. With kubeadm installed and ready to go, we'll bring up our first cluster. Run the following command: `kubeadm init --api-advertise-addresses "172.21.0.1"` (**Note**: The parameter name is planned to change in the future, so don't worry if it fails using `api-advertise-addresses`)
1. Join some nodes using the guides found in this directory
1. Run tests using the guides found in this directory
