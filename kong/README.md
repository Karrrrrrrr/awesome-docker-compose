# Kong

Kong 是一个云原生的 API 网关。

## 版本

- `kong3/`: Kong 3.x + Konga

## 快速启动

```bash
cd kong3 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8000 | Proxy (HTTP) |
| 8443 | Proxy (HTTPS) |
| 8001 | Admin API |
| 8444 | Admin HTTPS |
| 1337 | Konga Dashboard |

## 组件

| 服务 | 说明 |
|------|------|
| kong-db | PostgreSQL数据库 |
| kong | Kong网关 |
| konga | Web管理界面 |

## Konga Dashboard

访问 http://localhost:1337

首次使用需要:
1. 创建管理员账户
2. 配置Kong连接 (http://kong:8001)

## 常用命令

```bash
# 检查状态
curl http://localhost:8001/status

# 查看服务
curl http://localhost:8001/services

# 查看路由
curl http://localhost:8001/routes

# 查看消费者
curl http://localhost:8001/consumers

# 查看插件
curl http://localhost:8001/plugins
```

## 配置示例

### 添加服务

```bash
curl -i -X POST http://localhost:8001/services \
  -d "name=example-service" \
  -d "url=http://httpbin.org"
```

### 添加路由

```bash
curl -i -X POST http://localhost:8001/services/example-service/routes \
  -d "paths[]=/example"
```

### 添加插件

```bash
# 限流
curl -i -X POST http://localhost:8001/services/example-service/plugins \
  -d "name=rate-limiting" \
  -d "config.minute=100"

# JWT认证
curl -i -X POST http://localhost:8001/services/example-service/plugins \
  -d "name=jwt"

# CORS
curl -i -X POST http://localhost:8001/services/example-service/plugins \
  -d "name=cors" \
  -d "config.origins=*"
```

### 测试

```bash
curl http://localhost:8000/example/get
```

## 常用插件

| 插件 | 说明 |
|------|------|
| rate-limiting | 限流 |
| jwt | JWT认证 |
| key-auth | Key认证 |
| basic-auth | 基础认证 |
| cors | 跨域 |
| request-transformer | 请求转换 |
| response-transformer | 响应转换 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| KONG_PROXY_PORT | 8000 |
| KONG_ADMIN_PORT | 8001 |
| KONGA_PORT | 1337 |
| KONG_DB_PASSWORD | kong123456 |

## 持久化

- kong_db_data: 数据库数据
