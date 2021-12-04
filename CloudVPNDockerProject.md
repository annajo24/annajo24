# Anna Jordan Cloud VPN-Docker Project
## Set Up
Create Digital Ocean account \
Create Droplet 
- Ubuntu 20.04
- Basic
- Regular Internal CPU   
- New York Data Center

Create Root Password 
## Install Docker 

Launch Droplet Console \
Install essenatial files
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add repository
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
```
apt-cache policy docker-ce
```
Install docker
```
sudo apt install docker-ce -y
```
## Install Docker Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
Set docker-compose to execute
```
sudo chmod +x /usr/local/bin/docker-compose
```
## Set Up WireGuard
```
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml
```
Copy contents into yml file
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```
Modify file so that:
- TZ=America/Chicago
- SERVERURL=10.116.0.2

CTL + X, Y, ENTER to save the file
## Start Wireguard
```
cd ~/wireguard/
docker-compose up -d
```
Get QR Code for phone testing
```
docker-compose logs -f wireguard
```
