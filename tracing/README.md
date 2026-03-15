# 链路追踪

分布式链路追踪服务集合，用于监控和排查微服务架构中的调用链路。

## 服务列表

| 服务 | 端口 | 说明 |
|------|------|------|
| Jaeger | 16686 (UI) | 云原生分布式追踪 |
| Zipkin | 9411 | Twitter 开源分布式追踪 |
| SkyWalking | 8080 (UI), 11800 (gRPC) | APM 应用性能监控 |

## 快速启动

```bash
# Jaeger
cd jaeger && docker-compose up -d

# Zipkin
cd zipkin && docker-compose up -d

# SkyWalking
cd skywalking && docker-compose up -d
```

## 对比

| 特性 | Jaeger | Zipkin | SkyWalking |
|------|--------|--------|------------|
| 开发语言 | Go | Java | Java |
| OpenTelemetry | 原生支持 | 支持 | 支持 |
| APM 功能 | 基础 | 基础 | 完整 |
| 拓扑图 | 无 | 无 | 自动生成 |
| 告警 | 无 | 无 | 支持 |
| 学习曲线 | 简单 | 简单 | 中等 |

## 选择建议

- **Jaeger**: 云原生环境，Kubernetes 部署
- **Zipkin**: 简单场景，快速上手
- **SkyWalking**: 需要完整 APM 功能，告警，拓扑图
