# openvpn
Create site to site VPN using openvpn

# Create a docker image from this Dockerfile
docker build --tag openvpn .

# Sample site to site VPN
## Assumptions
openvpn server          : on cloud
openvpn client          : local server

Cloud Server private IP : 10.2.0.4
Cloud server public IP  : 20.33.100.22
cloud server hostname   : bramdocker.westus.cloudapp.azure.com

local client private IP : 192.168.0.17
local client public IP  : 202.111.222.11
local client hostname   : bramlocal

openvpn Port            : 1194
openvpn protocol        : udp
openvpn encryption      : TLS 1.3
openvpn certificate     : self-signed 2048 bits

SSH from port from openvpn client to openvpn server must be opened (port 22), this is required to perform automatic registration and installation of a self-signed certificate

## Pre-requisites
Required port to open client to server  : 22 (TCP)
Required port to open client <=> server : 1194 (UDP) <BR>
NOTE: both local->cloud and cloud->local

# How to Run
## Server must run first (in this case on-cloud)
docker run -it --name openvpn --net host -v /opt/openvpn:/etc/openvpn --cap-add=NET_ADMIN --device /dev/net/tun:/dev/net/tun openvpn server bramlocal<BR>
NOTE: press Ctrl+pq (to detach from docker container)

## Client can only be run when openvpn server is already running
docker run -it --name openvpn --net host -v /opt/openvpn:/etc/openvpn --cap-add=NET_ADMIN --device /dev/net/tun:/dev/net/tun openvpn client bramdocker.westus.cloudapp.azure.com<BR>
NOTE: press Ctrl+pq (to detach from docker container)

# How to Verify
ping private-ip from local to the cloud and vice versa
ping private-ip on the same subnet from local to the cloud and vice versa

