# SkyWalking

Apache SkyWalking 是一个开源的 APM（应用性能监控）系统，提供分布式追踪、服务网格遥测分析、度量聚合和可视化一体化解决方案。

## 快速启动

```bash
cd skywalking && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8080 | Web UI |
| 11800 | gRPC 接收端口 |
| 12800 | HTTP 接收端口 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| SKYWALKING_UI_PORT | 8080 |
| SKYWALKING_GRPC_PORT | 11800 |
| SKYWALKING_HTTP_PORT | 12800 |

## 访问界面

启动后访问 http://localhost:8080 查看 SkyWalking UI。

## 架构

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Agent     │────▶│     OAP     │────▶│     UI      │
│  (服务端)   │     │   (分析)    │     │  (可视化)   │
└─────────────┘     └──────┬──────┘     └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │   Storage   │
                    │  (存储后端) │
                    └─────────────┘
```

## 使用示例

### Java Agent

```bash
# 下载 SkyWalking Agent
wget https://archive.apache.org/dist/skywalking/java-agent/9.0.0/apache-skywalking-java-agent-9.0.0.tgz

# 启动应用时添加 Agent
java -javaagent:/path/to/skywalking-agent.jar \
     -Dskywalking.agent.service_name=my-service \
     -Dskywalking.collector.backend_service=localhost:11800 \
     -jar my-app.jar
```

### Spring Boot

```yaml
# application.yml
skywalking:
  agent:
    service-name: my-service
  collector:
    backend-service: localhost:11800
```

### Python

```python
from skywalking import agent, config

config.init(
    agent_name='my-service',
    collector_address='localhost:11800',
)

agent.start()

# 使用装饰器追踪
from skywalking.decorators import trace

@trace()
def my_function():
    pass
```

### Go

```go
import (
    "github.com/SkyAPM/go2sky"
    "github.com/SkyAPM/go2sky/reporter"
)

func initTracer() (*go2sky.Tracer, error) {
    rep, err := reporter.NewGRPCReporter("localhost:11800")
    if err != nil {
        return nil, err
    }

    tracer, err := go2sky.NewTracer("my-service", go2sky.WithReporter(rep))
    if err != nil {
        return nil, err
    }
    return tracer, nil
}
```

### Node.js

```javascript
const { Agent } = require('skywalking-backend-js')

const agent = new Agent({
  serviceName: 'my-service',
  collectorAddress: 'localhost:11800',
})
```

## 存储后端

默认使用 H2 内存数据库，生产环境建议：

- Elasticsearch
- MySQL
- TiDB
- PostgreSQL
- BanyanDB

### Elasticsearch 配置

```yaml
environment:
  - SW_STORAGE=elasticsearch
  - SW_STORAGE_ES_CLUSTER_NODES=elasticsearch:9200
```

## 功能特点

- **分布式追踪**: 追踪服务间调用链路
- **服务拓扑图**: 自动生成服务依赖关系图
- **性能指标**: 监控服务响应时间、成功率等
- **告警**: 支持多种告警规则和通知方式
- **服务网格**: 支持 Istio、Envoy 等

## 与其他 APM 对比

| 特性 | SkyWalking | Jaeger | Zipkin |
|------|------------|--------|--------|
| APM | 是 | 部分 | 部分 |
| 拓扑图 | 自动生成 | 无 | 无 |
| 告警 | 支持 | 无 | 无 |
| 服务网格 | 支持 | 支持 | 部分 |
| 语言支持 | Java, Go, Python, Node.js, PHP 等 | 多语言 | 多语言 |

## 健康检查

```bash
# OAP 健康检查
curl http://localhost:12800/healthcheck

# UI 健康检查
curl http://localhost:8080
```

## 相关链接

- 官网: https://skywalking.apache.org/
- GitHub: https://github.com/apache/skywalking
- 文档: https://skywalking.apache.org/docs/
