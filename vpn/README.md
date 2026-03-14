# VPN / 内网穿透

VPN和内网穿透服务。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| wireguard/ | Wireguard | 51820/UDP | VPN服务 |
| frp/ | FRP | 7000, 7500 | 内网穿透 |

## Wireguard

现代、高效的 VPN 协议。

### 端口

| 端口 | 协议 | 说明 |
|------|------|------|
| 51820 | UDP | VPN端口 |

### 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| WIREGUARD_URL | auto | 服务器地址 |
| WIREGUARD_PORT | 51820 | VPN端口 |
| WIREGUARD_PEERS | peer1,peer2 | 客户端名称 |
| PUID | 1000 | 用户ID |
| PGID | 1000 | 用户组ID |

### 客户端配置

启动后，客户端配置文件位于 `./config/peer1/` 目录。

### 连接

1. 扫描二维码或导入配置文件
2. 使用 Wireguard 客户端连接

## FRP (Fast Reverse Proxy)

高性能反向代理，用于内网穿透。

### 组件

- frps: 服务端 (公网服务器)
- frpc: 客户端 (内网服务器)

### 端口

| 端口 | 说明 |
|------|------|
| 7000 | frp通信端口 |
| 7500 | Dashboard |
| vhost_http_port | HTTP代理端口 |
| vhost_https_port | HTTPS代理端口 |

### 配置文件

```ini
# frps.ini (服务端)
[common]
bind_port = 7000
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin123456
token = your_token_here

# frpc.ini (客户端)
[common]
server_addr = your-server-ip
server_port = 7000
token = your_token_here

[web]
type = http
local_ip = 127.0.0.1
local_port = 80
custom_domains = your-domain.com
```

### 客户端配置示例

```ini
# SSH穿透
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6022

# Web穿透
[web]
type = http
local_ip = 127.0.0.1
local_port = 80
custom_domains = app.yourdomain.com
```

## 持久化

- wireguard_config: Wireguard配置
- frp_config: FRP配置
