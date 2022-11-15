## Start by creating a Digital Ocean account
- make account with the 200 credit
- set up a team(this took forever for some reason)
- make a new droplet:
	- Region: San Francisco
	- OS: Ubuntu 20.04 (LTS) x64
	- Size: Basic $7/mo AMD
	- Authentication Method Password: .Cane1Reign
	- all other options were left default
	- create!
- get into web console of droplet
## Installing docker
- Install tools for docker
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
- Adding a docker key
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- adding docker repository (for my 64bit linux OS)
```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
- switching to correct repo
```bash
apt-cache policy docker-ce
```
- Install Docker 
```bash
sudo apt install docker-ce -y
```
- Install Docker-Compose
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
- Set perms
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
- this part of the documentation was completed with the help of https://thematrix.dev/install-docker-and-docker-compose-on-ubuntu-20-04/
## Setup Wireguard
- make directory for wireguard
```bash
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
```
- nano into wireguard config
```bash
nano ~/wireguard/docker-compose.yml
```
- paste following yml config into file
```yml
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - SERVERURL=137.184.236.98
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
- this config was changed to the correct timezone, serverurl, and peers.
- serverurl was given from the ipv4 of the droplet
- start wireguard
```bash
cd ~/wireguard/
docker-compose up -d
```
- connect mobile device to wireguard:
```bash
docker-compose logs -f wireguard
```
- download wireguard app on mobile device
- scan qr code and name the tunnel
- screenshots here for documentation purposes

- find config file in wireguard
```bash
cd ~/wireguard/configs/peer_pc1
nano peer_pc1.conf
```
- copy config for desktop wireguard
- show working wireguard
- screenshots for documentation

- This part of the documentation was made with the help of https://thematrix.dev/setup-wireguard-vpn-server-with-docker/