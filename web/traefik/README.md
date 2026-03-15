# Traefik

Traefik 是一个现代的 HTTP 反向代理和负载均衡器。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `traefik3/` | 3.x | 最新版本 |
| `traefik2/` | 2.x | 稳定版本 |

## 重要变更

Traefik 3.x 与 2.x 配置有差异:
- 3.x: 使用新的路由语法
- 2.x: 传统配置方式

## 端口

| 端口 | 说明 |
|------|------|
| 80 | HTTP |
| 443 | HTTPS |
| 8080 | Dashboard |

## 快速启动

```bash
cd traefik3 && docker-compose up -d
```

## Dashboard

访问 http://localhost:8080

## 配置文件

- 主配置: `traefik.yml` (命令行参数)
- 动态配置: `./dynamic/` 目录

## 路由示例

### Docker Labels

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.myapp.rule=Host(`myapp.localhost`)"
  - "traefik.http.routers.myapp.entrypoints=web"
```

### 动态配置文件

```yaml
# dynamic/routes.yml
http:
  routers:
    myapp:
      rule: "Host(`myapp.localhost`)"
      service: myapp
      entryPoints:
        - web
  services:
    myapp:
      loadBalancer:
        servers:
          - url: "http://myapp:8080"
```

## Let's Encrypt

取消注释 `docker-compose.yml` 中的证书解析器配置:

```yaml
command:
  - "--certificatesresolvers.letsencrypt.acme.tlschallenge=true"
  - "--certificatesresolvers.letsencrypt.acme.email=admin@example.com"
  - "--certificatesresolvers.letsencrypt.acme.storage=/etc/traefik/acme.json"
```

然后在路由中启用:

```yaml
labels:
  - "traefik.http.routers.myapp.tls=true"
  - "traefik.http.routers.myapp.tls.certresolver=letsencrypt"
```

## 健康检查

```bash
curl http://localhost:8080/api/overview
```

## 持久化

- `acme.json`: SSL证书存储 (需要手动创建)
- `traefik_logs`: 访问日志
