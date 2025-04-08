---
Created: 2024-12-07T07:47:47+05:30
Updated: 2025-04-08T06:48:28+05:30
Maintainer: Ibrar Ansari
---
# Nginx Proxy Manager Load-Balancer Setup Guide

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/meibraransari/nginx-proxy-manager/blob/main/assets/npm.png">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/meibraransari/nginx-proxy-manager/blob/main/assets/npm.png">
    <img src="https://github.com/meibraransari/nginx-proxy-manager/blob/main/assets/npm.png" width="270" height="270" alt="NPM">
  </picture>
    <br>
    <strong>Nginx Proxy Manager</strong>
</p>


## 🎬 Nginx Proxy Manager Complete Playlist
[![Watch on Youtube](https://i.ytimg.com/vi/rxmEFm7EPck/maxresdefault.jpg)](https://www.youtube.com/playlist?list=PL5Afhqcc17s2UCcuEyFnTMHbVkxl8EG_7)

### Configuration Guide

#### Step 1. Add upstream in http.conf file
###### Inspect Nginx Proxy Manager Mount Path

```
CONATINER=npm
docker exec $CONATINER pwd
docker inspect $CONATINER | grep -i -A 17 "Mounts"
```
###### Create Custom Directory
```
mkdir -p /root/npm/nginx-proxy-manager/data/nginx/custom
ls /root/npm/nginx-proxy-manager/data/nginx/custom

```
###### Create upstream backend
```
nano /root/npm/nginx-proxy-manager/data/nginx/custom/http.conf
```
```
# Upstream for backend 1
upstream backend {
    server 192.168.1.104:8088 weight=2; #Nginx
    server 192.168.1.104:8081 weight=2; #UM
    server 192.168.1.104:8082 weight=2; #CS
    server 192.168.1.104:8090 weight=4; #Web
}

# Upstream for backend 2
#upstream backend2 {
#    server 192.168.192.222:8000;
#    server 192.168.192.222:9000;
#}

# You can add multiple backend with custom name here as shown above....
```

###### Verify upstream backend
```
docker exec $CONATINER ls -lash /data/nginx/
docker exec $CONATINER ls -lash /data/nginx/custom/
docker exec $CONATINER cat /data/nginx/custom/http.conf
```
##### Other upstream Examples:
For more information, see the [Upstream](../assets/upstream.md) file.


#### Steps 2. Include upstream file path in nginx.conf
##### Check and add http.conf file path in nginx.conf if not present
```
docker exec $CONATINER cat /etc/nginx/nginx.conf
```

##### If not present then add 
```
docker exec -it $CONATINER /bin/bash
apt update
apt install nano -y
nano /etc/nginx/nginx.conf
```

##### Include custom backend config file in http block in the last
```
include /data/nginx/custom/http[.]conf
```

##### Exit from the container
```
exit
```



#### Steps 3. Add domain in NPM

##### Add domain in Nginx proxy manager
In my case I am adding demo domain like "alb.devopsinaction.lab" in nginx proxy manager
NPM > Host > Proxy Host > Add Proxy Host
Domain: alb.devopsinaction.lab
Forward Hostname / IP*: 127.0.0.1 #no-use-of-it-in-lb-case
Forward Port *:  80 #no-use-of-it-in-lb-case




---
### 💼 Connect with me 👇👇 😊

- 🔥 [**Youtube**](https://www.youtube.com/@DevOpsinAction?sub_confirmation=1)
- ✍ [**Blog**](https://ibraransari.blogspot.com/)
- 💼 [**LinkedIn**](https://www.linkedin.com/in/ansariibrar/)
- 👨‍💻 [**Github**](https://github.com/meibraransari?tab=repositories)
- 💬 [**Telegram**](https://t.me/DevOpsinActionTelegram)
- 🐳 [**Docker**](https://hub.docker.com/u/ibraransaridocker)
