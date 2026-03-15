# Zipkin

Zipkin 是一个开源的分布式追踪系统，用于收集服务间调用的时序数据，帮助排查延迟问题。

## 快速启动

```bash
cd zipkin && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 9411 | Web UI & API |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| ZIPKIN_PORT | 9411 |

## 访问界面

启动后访问 http://localhost:9411 查看 Zipkin UI。

## 数据存储

默认使用内存存储，生产环境建议配置：

- Elasticsearch
- MySQL
- Cassandra
- Kafka

### Elasticsearch 配置示例

```yaml
environment:
  - STORAGE_TYPE=elasticsearch
  - ES_HOSTS=elasticsearch:9200
```

## 使用示例

### Spring Boot

```yaml
# application.yml
spring:
  zipkin:
    base-url: http://localhost:9411
    sender:
      type: web
  sleuth:
    sampler:
      probability: 1.0
```

### Go

```go
import (
    "github.com/openzipkin/zipkin-go"
    "github.com/openzipkin/zipkin-go/reporter/http"
)

func initTracer() (*zipkin.Tracer, error) {
    reporter := http.NewReporter("http://localhost:9411/api/v2/spans")
    defer reporter.Close()

    tracer, err := zipkin.NewTracer(reporter)
    if err != nil {
        return nil, err
    }
    return tracer, nil
}
```

### Python

```python
from py_zipkin.zipkin import zipkin_span

@zipkin_span(service_name='my-service', span_name='my-operation')
def my_function():
    # your code here
    pass
```

## API 示例

```bash
# 查询服务列表
curl http://localhost:9411/api/v2/services

# 查询服务依赖
curl http://localhost:9411/api/v2/dependencies?endTs=1640000000000

# 查询追踪
curl "http://localhost:9411/api/v2/traces?serviceName=my-service&limit=10"
```

## 与 Jaeger 对比

| 特性 | Zipkin | Jaeger |
|------|--------|--------|
| 开发语言 | Java | Go |
| 存储后端 | ES, MySQL, Cassandra, Kafka | ES, Cassandra, Kafka |
| 协议 | Scribe, HTTP, Kafka | UDP, HTTP, gRPC |
| UI | 简洁 | 更丰富 |
| OpenTracing | 支持 | 原生支持 |

## 健康检查

```bash
curl http://localhost:9411/health
```

## 相关链接

- 官网: https://zipkin.io/
- GitHub: https://github.com/openzipkin/zipkin
- 文档: https://zipkin.io/pages/instrumenting.html
