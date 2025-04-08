---
Created: 2024-12-07T07:47:47+05:30
Updated: 2025-04-08T06:36:41+05:30
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

##### Other upstream Examples:
For more information, see the [Upstream](../assets/upstream.md) file.


#### 2. Using Docker Compose
##### Create compose file
```
nano compose.yml
```

```
services:
  npm:
    image: jc21/nginx-proxy-manager:latest
    container_name: npm
    hostname: npm
    restart: unless-stopped
    environment:
      - DISABLE_IPV6 = 'true'
      - TZ=TZ=Asia/Kolkata
      - PUID=1000 # see https://nginxproxymanager.com/advanced-config/
      - PGID=1000 # see https://nginxproxymanager.com/advanced-config/
    ports:
      - 80:80/tcp # HTTP
      - 443:443/tcp # HTTPS
      - 81:81/tcp # MGMT UI, do not expose publicly
    dns:
      - 8.8.8.8
      - 8.8.4.4
    healthcheck:
      test: ["CMD", "/bin/check-health"]
      interval: 30s
      timeout: 3s
    volumes:
      - ./nginx-proxy-manager/data:/data
      - ./nginx-proxy-manager/letsencrypt:/etc/letsencrypt
```

##### Run container
```
docker-compose up -d
```

##### Access NPM Server
```
http://your_ip_or_FQDN:81
admin@example.com
changeme
```


---
### 💼 Connect with me 👇👇 😊

- 🔥 [**Youtube**](https://www.youtube.com/@DevOpsinAction?sub_confirmation=1)
- ✍ [**Blog**](https://ibraransari.blogspot.com/)
- 💼 [**LinkedIn**](https://www.linkedin.com/in/ansariibrar/)
- 👨‍💻 [**Github**](https://github.com/meibraransari?tab=repositories)
- 💬 [**Telegram**](https://t.me/DevOpsinActionTelegram)
- 🐳 [**Docker**](https://hub.docker.com/u/ibraransaridocker)
