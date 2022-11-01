---
title: Docker Project Pihole
layout: jekyll-theme-minimal
filename: DockerProject2.md
--- 
## Start by installing docker
This was showcased in the slides

## Setting up pihole
Open the terminal ssh into VM and start docker
```bash
sudo systemctl start docker
```
Download pihole from the docker website
```bash
sudo docker pull pihole/pihole
```
copy the yml file from the web for pihole
```yml
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
      - "443:443/tcp"
    environment:
      TZ: 'Asia/Kolkata' #this is the time zone
    volumes:
       - './etc-pihole/:/etc/pihole/'
       - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
    dns:
      - 127.0.0.1
      - 1.1.1.1
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```
create and edit file docker-compose.yml
```bash
nano docker-compose.yml
```
paste the yml code into this file

start up the yml to run pihole
```bash
sudo docker compose up -d
```
move into the pihole container
```bash
sudo docker exec -it pihole bash
```
once in the pihole container change the password
```bash
pihole -a -p
```
exit the container
```bash
exit
```
go to browser and search the ip associtated with the vm
```
10.10.1.129
```
redirect to the admin page and login with password you created
![[console to show running pihole.png]]
![[pihole ip.png]]
![[pihole admin.png]]
if you were to use pihole for permanent use change your DNS to pihole.

used a guide for this project the source of this guide is:
https://www.geeksforgeeks.org/create-your-own-secure-home-network-using-pi-hole-and-docker/
