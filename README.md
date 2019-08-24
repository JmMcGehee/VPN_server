# VPN_server
VPN server with a client side dashboard

1. Worked back OpenVPN folders.
2. Install Raspian Lite.
3. Install UFW.
4. Set up superuser:  https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04
5. Set up OpenVPN: https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-18-04#step-1-%E2%80%94-installing-openvpn-and-easyrsa
6. Make Client Portal
7. Add Auth
8. Add Metrics.


### Set up a superuser with reduced scope

-SSH Keys vs. password authentication for root access?

### Set up Uncomplicated Firewall.

-install ufw

### Install OpenVPN

### Writing Keys

```
./easyrsa init-pki
./easyrsa build-ca
./easyrsa gen-req server nopass
./easyrsa sign-req server server
 openssl dhparam -out dh2048.pem 2048
```

### Writing Client Keys

-Make client directory
-Generate client key/req
-Sign client key/req
-Send cert to client config file

### Configure OpenVPN Service

-Add an HMAC Firewall

### Tweak Networking Configuration

- IP forwarding
- Configure UFW
  - find the ip address: `ip route | grep default`

### Start the Server

### Create Client Configuration Infrastructure 

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

check status

`systemctl status openvpn@server`
`service openvpn status`

get errors:

`openvpn --config /etc/openvpn/server.conf`
