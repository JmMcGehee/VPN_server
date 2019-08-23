# VPN_server
VPN server with a client side dashboard

## 1. Server on Raspberry Pi
  - Public IP address
  - Dynamic DNS
  - Serving dynamic webpages
  - Client side dashboard

## 2. VPN
  - OpenVPN
  - OS: Ubuntu
  - Port forwarding?

## 3. Client side dashboard
  - OAuth?
  - Server metrics;

### I. Installing OpenVPN:

###### 1. Update the Raspberry Pi
```
sudo apt-get update
sudo apt-get upgrade
```
[source](https://www.ovpn.com/en/guides/raspberry-pi-raspbian)

-> SSH Into Raspberry Pi
-> Change root password: https://www.youtube.com/watch?v=v0vrkMP27PA
-> Set up vpn: https://www.youtube.com/watch?v=XcsQdtsCS1U&t=228s

Install Uncomplicated Firewall:
```
sudo apt install ufw
```

# Enable NAT and IP masquerading for clients
nano /etc/ufw/before.rules
# Add the following near the top
```
*nat
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING -s 10.8.0.0/8 -o eth0 -j MASQUERADE
COMMI
```
- `eht0` may be changed out for `wlan0`?

## Dashboard data

RX packets 3267  bytes 656113 (640.7 KiB)
RX errors 0  dropped 0  overruns 0  frame 0
TX packets 1420  bytes 207695 (202.8 KiB)
TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

### Writing Keys

```
./easyrsa init-pki
./easyrsa build-ca
./easyrsa gen-req server nopass
./easyrsa sign-req server server
 openssl dhparam -out dh2048.pem 2048
```

check status

`systemctl status openvpn@server`
`service openvpn status`

get errors:

`openvpn --config /etc/openvpn/server.conf`
