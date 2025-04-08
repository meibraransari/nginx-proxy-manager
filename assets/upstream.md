---
Created: 2025-04-08T06:30:16+05:30
Updated: 2025-04-08T06:31:59+05:30
---
#### Other Upstream Examples (Load balancing methods)
```
# Other examples
upstream backend {
    server 192.168.192.221:7000 primary;
    server 192.168.192.221:3020 backup; # or whatever is your config
}

upstream backend_servers {
    # Define your backend servers
    #least_conn;
    #ip_hash;
    server 192.168.1.101:80;  # Server 1
    server 192.168.1.102:80 ;  # Server 2
    server 192.168.1.103:80;  # Server 3
    check interval=5000 rise=2 fall=3 timeout=2000;
}

# Least Connections:
upstream backend_servers {
    least_conn;
    server 192.168.1.101:80 weight=5;
    server 192.168.1.102:80 max_fails=3 fail_timeout=30s;
    server 192.168.1.103:80;
}

# IP Hash (for session persistence):
upstream backend_servers {
    ip_hash;
    server 192.168.1.101:80;
    server 192.168.1.102:80;
    server 192.168.1.103:80;
}

# Weighted Round Robin:
upstream backend_servers {
    server 192.168.1.101:80 weight=3;  # 60% of traffic
    server 192.168.1.102:80 weight=1;  # 20% of traffic
    server 192.168.1.103:80 weight=1;  # 20% of traffic
}

```

#### References

https://nginx.org/en/docs/http/load_balancing.html
