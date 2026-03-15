# Jaeger

Jaeger 是一款开源的、端到端的分布式链路追踪系统，用于监控和排查微服务架构中的事务。

## 快速启动

```bash
cd jaeger && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 16686 | Web UI |
| 14268 | HTTP Collector |
| 14250 | gRPC Collector |
| 9411 | Zipkin 兼容 |
| 4317 | OTLP gRPC |
| 4318 | OTLP HTTP |
| 6831/udp | Jaeger Agent (compact) |
| 6832/udp | Jaeger Agent (binary) |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| JAEGER_UI_PORT | 16686 |
| JAEGER_COLLECTOR_HTTP_PORT | 14268 |
| JAEGER_COLLECTOR_GRPC_PORT | 14250 |
| JAEGER_ZIPKIN_PORT | 9411 |
| JAEGER_OTLP_GRPC_PORT | 4317 |
| JAEGER_OTLP_HTTP_PORT | 4318 |

## 访问界面

启动后访问 http://localhost:16686 查看 Jaeger UI。

## 使用示例

### Go (Jaeger Client)

```go
import (
    "github.com/opentracing/opentracing-go"
    "github.com/uber/jaeger-client-go"
    jaegercfg "github.com/uber/jaeger-client-go/config"
)

func initTracer() (opentracing.Tracer, io.Closer, error) {
    cfg := jaegercfg.Configuration{
        ServiceName: "my-service",
        Sampler: &jaegercfg.SamplerConfig{
            Type:  jaeger.SamplerTypeConst,
            Param: 1,
        },
        Reporter: &jaegercfg.ReporterConfig{
            LocalAgentHostPort: "localhost:6831",
        },
    }
    return cfg.NewTracer()
}
```

### Go (OpenTelemetry)

```go
import (
    "go.opentelemetry.io/otel"
    "go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc"
    "go.opentelemetry.io/otel/sdk/trace"
)

func initTracer() (*trace.TracerProvider, error) {
    exporter, err := otlptracegrpc.New(context.Background(),
        otlptracegrpc.WithEndpoint("localhost:4317"),
        otlptracegrpc.WithInsecure(),
    )
    if err != nil {
        return nil, err
    }
    tp := trace.NewTracerProvider(trace.WithBatcher(exporter))
    otel.SetTracerProvider(tp)
    return tp, nil
}
```

### Python

```python
from jaeger_client import Config

def init_tracer(service_name='my-service'):
    config = Config(
        config={
            'sampler': {'type': 'const', 'param': 1},
            'local_agent': {'reporting_host': 'localhost', 'reporting_port': 6831},
        },
        service_name=service_name,
    )
    return config.initialize_tracer()
```

### Java (Spring Boot)

```yaml
# application.yml
spring:
  application:
    name: my-service
opentracing:
  jaeger:
    http-sender:
      url: http://localhost:14268/api/traces
    probabilistic-sampler:
      probability: 1.0
```

## 数据采集方式

### 1. Jaeger Agent (UDP)

```bash
# 应用通过 UDP 发送到 Agent
# Agent 地址: localhost:6831
```

### 2. HTTP Collector

```bash
curl -X POST http://localhost:14268/api/traces \
  -H "Content-Type: application/json" \
  -d @trace.json
```

### 3. OTLP (OpenTelemetry)

```bash
# gRPC
otel-collector -> jaeger:4317

# HTTP
otel-collector -> jaeger:4318
```

### 4. Zipkin 兼容

```bash
curl -X POST http://localhost:9411/api/v2/spans \
  -H "Content-Type: application/json" \
  -d @spans.json
```

## 架构说明

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Service A  │────▶│   Service B  │────▶│   Service C  │
└──────┬───────┘     └──────┬───────┘     └──────┬───────┘
       │                    │                    │
       └────────────────────┼────────────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Jaeger Agent  │ (UDP 6831)
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Jaeger        │
                    │ Collector     │ (14268/14250)
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Jaeger UI     │ (16686)
                    └───────────────┘
```

## 查询追踪

```bash
# 查看所有服务
curl http://localhost:16686/api/services

# 查看服务操作
curl http://localhost:16686/api/services/my-service/operations

# 查询追踪
curl "http://localhost:16686/api/traces?service=my-service&limit=20"
```

## 存储后端

默认使用内存存储，生产环境建议配置:

- Elasticsearch
- Cassandra
- Kafka
- Badger (嵌入式)

## 与其他追踪系统对比

| 特性 | Jaeger | Zipkin | SkyWalking |
|------|--------|--------|------------|
| 云原生 | 是 | 是 | 是 |
| OpenTracing | 是 | 是 | 部分 |
| OpenTelemetry | 是 | 是 | 是 |
| 性能 | 高 | 中 | 高 |
| UI | 简洁 | 简洁 | 丰富 |

## 健康检查

```bash
curl http://localhost:16686/
```

## 相关链接

- 官网: https://www.jaegertracing.io/
- GitHub: https://github.com/jaegertracing/jaeger
- 文档: https://www.jaegertracing.io/docs/
