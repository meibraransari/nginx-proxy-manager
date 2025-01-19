---
Created: 2024-12-07T07:47:47+05:30
Updated: 2024-12-07T08:44:33+05:30
Maintainer: Ibrar Ansari
---
# Nginx Proxy Manager Deployment using Docker Run & Docker Compose

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/meibraransari/nginx-proxy-manager/blob/main/assets/npm.png">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/meibraransari/nginx-proxy-manager/blob/main/assets/npm.png">
    <img src="https://github.com/meibraransari/nginx-proxy-manager/blob/main/assets/npm.png" width="270" height="270" alt="NPM">
  </picture>
    <br>
    <strong>Nginx Proxy Manager</strong>
</p>


### Nginx Proxy Manager?
Nginx Proxy Manager is a user-friendly interface for managing and configuring Nginx as a reverse proxy server. Nginx is a popular web server and reverse proxy used to handle requests, load balance, and direct traffic to different services or applications.

Official Documentation: https://nginxproxymanager.com/

<!---
## 🎬 Nginx Proxy Manager Setup Guide (Zero to Hero)
[![Watch on Youtube](https://i.ytimg.com/vi/FYuOSGJk7j0/maxresdefault.jpg)](https://youtu.be/FYuOSGJk7j0)

-->
### **Prerequisites**  
- Basic understanding of Docker.
- Docker must be installed on your system.
- Basic knowledge of command-line operations.

### Deployment Guide

#### 1. Using Docker run command

```
docker run -itd --name=c_nginx_proxy_manager --restart=always -p 80:80 -p 81:81 -p 443:443 -v $(pwd)/nginx-proxy-manager/data:/data -v $(pwd)/nginx-proxy-manager/letsencrypt:/etc/letsencrypt jc21/nginx-proxy-manager:latest
```
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
