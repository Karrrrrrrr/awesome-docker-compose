# 消息推送

消息推送通知服务。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| gotify/ | Gotify | 8080 | 消息推送服务 |
| ntfy/ | ntfy | 8080 | 推送通知服务 |

## Gotify

简单的消息推送服务器。

### 端口

| 端口 | 说明 |
|------|------|
| 8080 | Web界面/REST API |

### 使用

1. 访问 http://localhost:8080
2. 登录 (admin / admin123456)
3. 创建应用获取Token
4. 发送消息

### API示例

```bash
# 发送消息
curl "http://localhost:8080/message?token=<TOKEN>" \
  -F "title=Test" \
  -F "message=Hello World" \
  -F "priority=5"

# 获取消息
curl "http://localhost:8080/message?token=<TOKEN>"
```

### 环境变量

| 变量 | 默认值 |
|------|--------|
| GOTIFY_USER | admin |
| GOTIFY_PASSWORD | admin123456 |

## ntfy

简单的基于HTTP的发布-订阅通知服务。

### 端口

| 端口 | 说明 |
|------|------|
| 8080 | Web界面/HTTP API |

### 使用

```bash
# 发布消息
curl -d "Hello World" http://localhost:8080/mytopic

# 订阅消息
curl http://localhost:8080/mytopic/json

# Web界面订阅
# 访问 http://localhost:8080
```

### 特点

- 无需账户即可使用
- 支持Web界面
- REST API
- WebSocket支持
- 命令行工具

### 环境变量

| 变量 | 默认值 |
|------|--------|
| NTFY_BASE_URL | http://localhost:8080 |

## 对比

| 特性 | Gotify | ntfy |
|------|--------|------|
| 认证 | 必须 | 可选 |
| 持久化 | 是 | 是 |
| Web UI | 有 | 有 |
| 移动App | Android | Android/iOS |
| 自托管 | 简单 | 简单 |

## 持久化

- gotify_data: Gotify数据
- ntfy_cache: ntfy缓存
- ntfy_lib: ntfy数据
