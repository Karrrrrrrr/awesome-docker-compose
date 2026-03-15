# DTM (Distributed Transaction Manager)

DTM 是一款跨语言的开源分布式事务管理器，优雅地解决了幂等、空补偿、悬挂等分布式事务难题。

## 快速启动

```bash
cd dtm && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 36789 | HTTP API |
| 36790 | gRPC API |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| DTM_HTTP_PORT | 36789 |
| DTM_GRPC_PORT | 36790 |
| MYSQL_ROOT_PASSWORD | dtm123456 |

## 支持的事务模式

- **SAGA**: 长事务解决方案，支持补偿
- **TCC**: Try-Confirm-Cancel 模式
- **XA**: 数据库原生分布式事务
- **2PC**: 两阶段提交
- **Workflow**: 工作流模式

## 使用示例

### HTTP API

```bash
# 健康检查
curl http://localhost:36789/api/dtmsvr/ping

# 提交事务
curl -X POST http://localhost:36789/api/dtmsvr/submit \
  -H "Content-Type: application/json" \
  -d '{
    "gid": "test-gid",
    "trans_type": "saga",
    "steps": [
      {"action": "http://service-a/action", "compensate": "http://service-a/compensate"},
      {"action": "http://service-b/action", "compensate": "http://service-b/compensate"}
    ]
  }'
```

### Go SDK

```go
import (
    "github.com/dtm-labs/client"
    "github.com/dtm-labs/client/workflow"
)

// SAGA 事务
saga := dtmcli.NewSaga("http://localhost:36789/api/dtmsvr", "business-id")
saga.Add("http://service-a/action", "http://service-a/compensate", nil)
saga.Add("http://service-b/action", "http://service-b/compensate", nil)
err := saga.Submit()
```

### Python

```python
import requests

# 创建 SAGA 事务
response = requests.post(
    'http://localhost:36789/api/dtmsvr/submit',
    json={
        'gid': 'order-123',
        'trans_type': 'saga',
        'steps': [
            {'action': 'http://order-service/create', 'compensate': 'http://order-service/cancel'},
            {'action': 'http://payment-service/pay', 'compensate': 'http://payment-service/refund'}
        ]
    }
)
```

## 架构说明

```
┌─────────────────────────────────────────────────┐
│                   DTM Server                     │
│  ┌─────────┐  ┌─────────┐  ┌─────────────────┐ │
│  │ HTTP API│  │ gRPC API│  │ Transaction Mgr │ │
│  └─────────┘  └─────────┘  └─────────────────┘ │
└───────────────────────┬─────────────────────────┘
                        │
                        ▼
              ┌─────────────────┐
              │   MySQL Store   │
              └─────────────────┘
```

## 存储支持

- MySQL (默认)
- PostgreSQL
- Redis
- MongoDB

## 与 Seata 对比

| 特性 | DTM | Seata |
|------|-----|-------|
| 语言 | Go | Java |
| 跨语言 | 支持 | 有限 |
| SAGA | 支持 | 支持 |
| TCC | 支持 | 支持 |
| 性能 | 更高 | 较高 |

## 健康检查

```bash
curl http://localhost:36789/api/dtmsvr/ping
```

## 数据卷

- dtm_mysql_data: MySQL 数据存储

## 相关链接

- 官网: https://dtm.pub/
- GitHub: https://github.com/dtm-labs/dtm
