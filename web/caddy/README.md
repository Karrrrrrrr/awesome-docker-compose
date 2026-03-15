# Caddy

Caddy 是一个强大的、可扩展的服务器平台，自动管理HTTPS。

## 特点

- 自动HTTPS (Let's Encrypt)
- 简洁的配置语法
- HTTP/2 和 HTTP/3 支持
- 内置反向代理

## 版本

- `caddy2/`: Caddy 2.x

## 快速启动

```bash
cd caddy2 && docker-compose up -d
```

## 端口

| 端口 | 协议 | 说明 |
|------|------|------|
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 443 | UDP | HTTP/3 (QUIC) |

## Caddyfile

```caddyfile
# 静态站点
:80 {
    root * /srv
    file_server browse
}

# 反向代理
app.localhost {
    reverse_proxy app:8080
}

# 自动HTTPS
yourdomain.com {
    reverse_proxy app:8080
}
```

## 配置示例

### 静态文件

```caddyfile
:80 {
    root * /srv
    file_server browse
    encode gzip
}
```

### 反向代理

```caddyfile
api.localhost {
    reverse_proxy api-server:3000
}
```

### 负载均衡

```caddyfile
lb.localhost {
    reverse_proxy {
        to app1:8080
        to app2:8080
        to app3:8080
        lb_policy round_robin
    }
}
```

### PHP

```caddyfile
php.localhost {
    root * /srv
    php_fastcgi php:9000
    file_server
}
```

### 文件服务器

```caddyfile
files.localhost {
    root * /data
    file_server browse {
        hide .git .env
    }
}
```

### API网关

```caddyfile
api.localhost {
    api /users/* user-service:8080
    api /orders/* order-service:8080
    api /products/* product-service:8080
}
```

## 常用命令

```bash
# 验证配置
docker exec caddy caddy validate --config /etc/caddy/Caddyfile

# 重载配置
docker exec caddy caddy reload --config /etc/caddy/Caddyfile

# 格式化配置
docker exec caddy caddy fmt --overwrite /etc/caddy/Caddyfile

# 查看证书
docker exec caddy caddy list-certificates
```

## 环境变量

| 变量 | 默认值 |
|------|--------|
| CADDY_HTTP_PORT | 80 |
| CADDY_HTTPS_PORT | 443 |

## 持久化

- `./config`: Caddy配置
- `./data`: Caddy数据 (证书等)
- `./site`: 静态文件
