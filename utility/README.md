# 实用工具

各种实用工具服务。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| it-tools/ | IT-Tools | 8080 | IT人员工具集 |
| watchtower/ | Watchtower | - | 自动更新容器 |
| memos/ | Memos | 5230 | 备忘录/笔记 |
| outline/ | Outline | 3000 | 知识库Wiki |
| trilium/ | Trilium | 8080 | 知识管理/笔记 |
| hoppscotch/ | Hoppscotch | 3000 | API测试工具 |
| httpbin/ | HTTPBin | 8080 | HTTP测试服务 |
| traefik-whoami/ | Whoami | 8080 | 测试服务 |

## IT-Tools

在线IT工具集合。

### 功能

- 编码/解码: Base64, URL, HTML
- 哈希生成: MD5, SHA1, SHA256
- UUID生成
- JWT解析
- 正则测试
- 时间转换
- 二维码生成
- 等等...

### 访问

http://localhost:8080

## Watchtower

自动更新 Docker 容器。

### 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| WATCHTOWER_POLL_INTERVAL | 86400 | 检查间隔(秒) |
| WATCHTOWER_CLEANUP | true | 清理旧镜像 |
| WATCHTOWER_SCHEDULE | - | Cron格式定时 |

### 使用

```bash
# 每天检查更新
cd watchtower && docker-compose up -d

# 立即检查更新
docker exec watchtower /watchtower --run-once
```

## Memos

轻量级备忘录服务。

### 功能

- Markdown支持
- 标签分类
- 全文搜索
- 公开分享

### 访问

http://localhost:5230

## Outline

团队知识库/Wiki。

### 功能

- Markdown编辑
- 实时协作
- 权限管理
- 全文搜索

### 访问

http://localhost:3000

## Trilium

知识管理和笔记软件。

### 功能

- 层级笔记
- 代码高亮
- 关系图谱
- 端到端加密

### 访问

http://localhost:8080

## Hoppscotch

开源API测试工具 (Postman替代)。

### 功能

- REST API测试
- GraphQL支持
- WebSocket测试
- 环境变量
- 历史记录

### 访问

http://localhost:3000

## HTTPBin

HTTP请求测试服务。

### 端点

| 端点 | 说明 |
|------|------|
| /get | GET请求 |
| /post | POST请求 |
| /put | PUT请求 |
| /delete | DELETE请求 |
| /status/:code | 指定状态码 |
| /delay/:seconds | 延迟响应 |
| /headers | 返回请求头 |
| /ip | 返回IP |

### 使用

```bash
curl http://localhost:8080/get
curl -X POST -d "test=data" http://localhost:8080/post
curl http://localhost:8080/status/404
curl http://localhost:8080/delay/3
```

## 持久化

各服务数据存储在对应的 Docker volume 中。
