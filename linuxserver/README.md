# LinuxServer.io 服务

LinuxServer.io 提供了一系列优化的 Docker 镜像，特别适合 NAS 和家庭服务器。

## 特点

- 统一的 PUID/PGID 权限管理
- 活跃的社区支持
- 定期更新

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| code-server/ | VS Code Web | 8443 | 在线代码编辑器 |
| syncthing/ | Syncthing | 8384 | 文件同步 |
| nextcloud/ | Nextcloud | 444 | 私有云存储 |
| portainer/ | Portainer | 9000 | Docker管理界面 |
| uptime-kuma/ | Uptime Kuma | 3001 | 服务监控 |
| jellyfin/ | Jellyfin | 8096 | 媒体服务器 |
| filebrowser/ | FileBrowser | 8085 | 文件管理 |
| homeassistant/ | Home Assistant | 8123 | 智能家居 |
| n8n/ | n8n | 5678 | 工作流自动化 |
| vaultwarden/ | Vaultwarden | 8123 | 密码管理器 |
| nginx/ | Nginx | 80, 443 | Web服务器 |
| redis/ | Redis | 6379 | 缓存 |

## 通用环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| PUID | 1000 | 用户ID |
| PGID | 1000 | 用户组ID |
| TZ | Asia/Shanghai | 时区 |

## 快速启动

```bash
# 启动VS Code Web
cd code-server && docker-compose up -d

# 启动私有云
cd nextcloud && docker-compose up -d

# 启动媒体服务器
cd jellyfin && docker-compose up -d
```

## 获取 PUID/PGID

```bash
# 获取当前用户ID
id -u
# 输出: 1000

# 获取当前用户组ID
id -g
# 输出: 1000
```

## 权限问题

如果遇到权限问题，确保:

1. PUID/PGID 与宿主机用户一致
2. 数据目录权限正确
3. 使用正确的用户运行 Docker

## 默认账号密码

| 服务 | 用户 | 密码 |
|------|------|------|
| Code Server | - | code123456 |
| Portainer | - | 首次访问设置 |
| Nextcloud | - | 首次访问设置 |
| Vaultwarden | - | 首次访问设置 |
| n8n | admin | admin123456 |
| Uptime Kuma | - | 首次访问设置 |
