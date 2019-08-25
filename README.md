# VPN_server: OpenVPN + Raspberry Pi

*Work in Progress*

This repo is from a recent foray of mine into network engineering, and shows some of the notes I collected while setting up a VPN server on a Raspberry Pi 4. I used OpenVPN on the Raspberry Pi and Tunnelblick on the client side to run the configuration files.

Though this process can be completed in a relatively short time, it can quickly become overcomplicated for people who are newcomers to Networking. I myself worked, rolled back, and reworked this process and it's components multiple times before success.

As the Raspberry Pi 4 was released barely 1 month before I attempted this project, there was very little documentation that matched many of the errors and "gotchas" I ran into.

In light of that, I will be re-writing this markdown on an on-going basis. My hope to better explain the whole process and fill the information gaps in such a way that will make learning and applying Network security more accessible to newcomers.

I will be supplying as many tutorials, resources and explanations as possible to help cut down on easy errors, as well as to limit the amount of time needed for research. By the end, I hope this will be a "start-to-finish" resource that I can keep up-to-date for anyone with no experience that wants to get started today.  

## Raspberry Pi 4

## Stuff you should know about IP addresses

## Commands we'll be using

## EasyRSA
##### What are keys, certs, reqs?

## SSH

## OpenVPN

## Firewalls

## Links to configuration files

## Index

1. Work back OpenVPN folders.
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
===============================================
### Writing Client Keys

-Make client directory
-Generate client key/req
```
cd ~/EasyRSA-v3.0.6/
./easyrsa gen-req client1 nopass
```
-Sign client key/req
`./easyrsa sign-req client client1`
-Send cert to client config file
-->Build scripts
```
```
cd ~/EasyRSA-v3.0.6/
./easyrsa gen-req client1 nopass
```
```
cd ~/EasyRSA-v3.0.6/
./easyrsa gen-req client1 nopass
```
cd ~/client-configs
sudo ./make_config.sh client1
```

===============================================
### Configure OpenVPN Service

-Add an HMAC Firewall

### Tweak Networking Configuration

- IP forwarding
- Configure UFW
  - find the ip address: `ip route | grep default`

### Start the Server

### Create Client Configuration Infrastructure

- Point clients towards the VPN server IP -- I'm guessing this needs to be the public IP address.
  - Find your public IP address `wget -qO- http://ipecho.net/plain | xargs echo`
- Make a build script to generate client files.

### Generate Client files
- Make sure the keys folder has the client keys.

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

check status

`systemctl status openvpn@server`
`service openvpn status`

get errors:

`openvpn --config /etc/openvpn/server.conf`
