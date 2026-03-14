# Nginx

高性能HTTP和反向代理服务器。

## 快速启动

```bash
cd nginx && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 80 | HTTP |
| 443 | HTTPS |

## 配置文件

| 文件 | 说明 |
|------|------|
| nginx.conf | 主配置文件 |
| conf.d/*.conf | 站点配置 |
| ssl/ | SSL证书 |
| html/ | 静态文件 |

## 主配置

```nginx
# nginx.conf
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 10240;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;
    gzip on;

    include /etc/nginx/conf.d/*.conf;
}
```

## 站点配置示例

### 静态站点

```nginx
# conf.d/static.conf
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 反向代理

```nginx
# conf.d/proxy.conf
server {
    listen 80;
    server_name app.localhost;

    location / {
        proxy_pass http://app:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 负载均衡

```nginx
# conf.d/lb.conf
upstream backend {
    server app1:8080;
    server app2:8080;
    server app3:8080;
}

server {
    listen 80;
    server_name lb.localhost;

    location / {
        proxy_pass http://backend;
    }
}
```

### PHP站点

```nginx
# conf.d/php.conf
server {
    listen 80;
    server_name php.localhost;
    root /var/www/html;
    index index.php index.html;

    location ~ \.php$ {
        fastcgi_pass php:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

## 常用命令

```bash
# 测试配置
docker exec nginx nginx -t

# 重载配置
docker exec nginx nginx -s reload

# 查看日志
docker logs -f nginx

# 进入容器
docker exec -it nginx sh
```

## SSL配置

```nginx
server {
    listen 443 ssl http2;
    server_name secure.localhost;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
}
```

## 持久化

- nginx_logs: 日志存储
