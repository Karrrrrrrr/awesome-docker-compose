# NPS

NPS 是一个轻量级的内网穿透代理服务器。

## 快速启动

```bash
cd nps && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 80 | HTTP代理 |
| 443 | HTTPS代理 |
| 8024 | Bridge (客户端连接) |
| 8080 | Web管理界面 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| NPS_HTTP_PORT | 80 |
| NPS_HTTPS_PORT | 443 |
| NPS_BRIDGE_PORT | 8024 |
| NPS_WEB_PORT | 8080 |

## Web管理界面

访问 http://localhost:8080

| 用户 | 密码 |
|------|------|
| admin | admin123456 |

## 配置文件

`./conf/nps.conf`

## 使用步骤

### 1. 服务端配置

1. 访问管理界面 http://localhost:8080
2. 登录后创建客户端
3. 记录客户端的 vkey

### 2. 客户端配置

下载NPC客户端: https://github.com/ehang-io/nps/releases

```bash
# Windows
npc.exe -server=<服务器IP>:8024 -vkey=<客户端密钥>

# Linux/Mac
./npc -server=<服务器IP>:8024 -vkey=<客户端密钥>
```

### 3. 创建隧道

在管理界面中:

**TCP隧道**:
- 模式: TCP
- 目标IP: 内网服务IP
- 目标端口: 内网服务端口
- 服务端端口: 外网访问端口

**HTTP隧道**:
- 模式: HTTP
- 目标IP: 内网服务IP
- 目标端口: 内网服务端口
- 域名: 自定义域名

**SOCKS5代理**:
- 模式: SOCKS5
- 服务端端口: 代理端口

## 隧道类型

| 类型 | 说明 |
|------|------|
| TCP | TCP端口转发 |
| UDP | UDP端口转发 |
| HTTP | HTTP代理 |
| HTTPS | HTTPS代理 |
| SOCKS5 | SOCKS5代理 |
| P2P | 点对点连接 |

## 示例场景

### SSH穿透

1. 创建TCP隧道
2. 目标: 127.0.0.1:22
3. 服务端口: 2222
4. 连接: `ssh -p 2222 user@server-ip`

### Web服务穿透

1. 创建HTTP隧道
2. 目标: 127.0.0.1:8080
3. 域名: myapp.example.com
4. 访问: http://myapp.example.com

### 数据库穿透

1. 创建TCP隧道
2. 目标: 127.0.0.1:3306
3. 服务端口: 13306
4. 连接: `mysql -h server-ip -P 13306`

## 常用命令

```bash
# 查看日志
docker logs -f nps

# 重启服务
docker restart nps

# 查看状态
docker exec nps nps status
```

## 配置示例

```ini
[common]
bind_port=8024
web_host=0.0.0.0
web_username=admin
web_password=admin123456
web_port=8080
http_proxy_port=80
https_proxy_port=443
public_vkey=12345678
```

## 持久化

- nps_conf: 配置文件
