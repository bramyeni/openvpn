# openvpn
Create a site to site VPN using openvpn

# Create a docker image from this Dockerfile
## Pre-requisites
docker must have been installed and configured, <BR> also the docker daemon is already running <BR>
copy the Dockerfile which from this repo to let say /opt or /root or any of your linux mountpoint

## Build docker image
cd /opt<BR>
docker build --tag openvpn .<BR>

## Verify the docker image
docker images<BR><BR>

NOTE: if the openvpn docker image is listed then, it is ready to run

# Sample site to site VPN
<pre>
## Assumptions
openvpn server          : on cloud<BR>
openvpn client          : local server<BR>

Cloud Server private IP : 10.2.0.4<BR>
Cloud server public IP  : 20.33.100.22<BR>
cloud server hostname   : bramdocker.westus.cloudapp.azure.com<BR>

local client private IP : 192.168.0.17<BR>
local client public IP  : 202.111.222.11<BR>
local client hostname   : bramlocal<BR>

openvpn Port            : 1194<BR>
openvpn protocol        : udp<BR>
openvpn encryption      : TLS 1.3<BR>
openvpn certificate     : self-signed 2048 bits<BR>

SSH from port from openvpn client to openvpn server must be opened (port 22), this is required to perform automatic registration and installation of a self-signed certificate<BR>
</pre>

## Pre-requisites
Required port to open client to server  : 22 (TCP)<BR>
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
ping private-ip from local to the cloud and vice versa<BR>
ping private-ip on the same subnet from local to the cloud and vice versa

