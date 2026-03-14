# Awesome Docker Compose

一套完整的 Docker Compose 模板集合，包含常用服务的开箱即用配置。

## 特点

- **版本化管理**: 每个服务按版本独立目录，方便选择特定版本
- **多版本支持**: 热门服务提供多个版本选择
- **完整配置**: 包含配置文件、健康检查、持久化存储
- **管理界面**: 大部分服务附带Web管理界面

## 目录结构

```
awesome-docker-compose/
├── docker-compose.yml              # 主集成文件
├── .env.example                    # 环境变量模板
├── README.md                       # 文档
│
├── mysql/
│   ├── mysql8/docker-compose.yml   # MySQL 8.0 + phpMyAdmin
│   └── mysql5/docker-compose.yml   # MySQL 5.7 + phpMyAdmin
│
├── postgres/
│   ├── postgres16/docker-compose.yml  # PostgreSQL 16 + pgAdmin
│   └── postgres14/docker-compose.yml  # PostgreSQL 14
│
├── mongo/
│   ├── mongo7/docker-compose.yml   # MongoDB 7 + Mongo Express
│   └── mongo6/docker-compose.yml   # MongoDB 6
│
├── redis/
│   ├── redis7/docker-compose.yml   # Redis 7 + Redis Commander
│   ├── redis6/docker-compose.yml   # Redis 6
│   └── redis-cluster/docker-compose.yml  # Redis Cluster (6节点)
│
├── rabbitmq/
│   └── rabbitmq3/docker-compose.yml  # RabbitMQ 3 + Management UI
│
├── kafka/
│   ├── kafka3/docker-compose.yml   # Kafka 3 + Zookeeper + Kafka UI
│   └── kafka-kraft/docker-compose.yml  # Kafka KRaft模式 (无Zookeeper)
│
├── elasticsearch/
│   ├── es8/docker-compose.yml      # Elasticsearch 8 + Kibana
│   └── es7/docker-compose.yml      # Elasticsearch 7 + Kibana
│
├── clickhouse/
│   └── clickhouse24/docker-compose.yml  # ClickHouse 24
│
├── neo4j/
│   └── neo4j5/docker-compose.yml   # Neo4j 5
│
├── arangodb/
│   └── arangodb3/docker-compose.yml  # ArangoDB 3
│
├── orientdb/
│   └── orientdb3/docker-compose.yml  # OrientDB 3
│
├── influxdb/
│   └── influxdb2/docker-compose.yml  # InfluxDB 2 + Telegraf
│
├── minio/
│   └── minio/docker-compose.yml    # MinIO 对象存储
│
├── nginx/
│   └── nginx/docker-compose.yml    # Nginx
│
├── traefik/
│   └── traefik3/docker-compose.yml # Traefik 3
│
├── caddy/
│   └── caddy2/docker-compose.yml   # Caddy 2
│
├── consul/
│   └── consul1/docker-compose.yml  # Consul 1
│
├── etcd/
│   └── etcd3/docker-compose.yml    # Etcd 3 + etcdkeeper
│
├── emqx/
│   └── emqx5/docker-compose.yml    # EMQX 5
│
├── nsq/
│   └── nsq/docker-compose.yml      # NSQ (nsqlookupd + nsqd + nsqadmin)
│
├── kong/
│   └── kong3/docker-compose.yml    # Kong 3 + Konga
│
├── openresty/
│   └── openresty/docker-compose.yml  # OpenResty
│
├── gitea/
│   └── gitea1/docker-compose.yml   # Gitea 1
│
├── gitlab/
│   └── gitlab-ce/docker-compose.yml  # GitLab CE
│
├── imgproxy/
│   └── imgproxy3/docker-compose.yml  # Imgproxy 3
│
├── nps/
│   └── nps/docker-compose.yml      # NPS 内网穿透
│
├── dragonfly/
│   └── dragonfly/docker-compose.yml  # Dragonfly (Redis替代)
│
├── rustfs/
│   └── rustfs/docker-compose.yml   # RustFS
│
├── debian/
│   └── debian12/docker-compose.yml # Debian 12
│
└── ubuntu/
    └── ubuntu22/docker-compose.yml # Ubuntu 22.04
```

## 快速开始

```bash
# 复制环境变量文件
cp .env.example .env

# 编辑环境变量（可选）
vim .env

# 方式1: 使用主文件启动服务组
docker-compose --profile database up -d
docker-compose --profile cache up -d
docker-compose --profile full up -d

# 方式2: 进入单独服务目录启动
cd mysql/mysql8
docker-compose up -d
```

## 服务分组（Profiles）

主 `docker-compose.yml` 使用 profiles 来分组服务：

| Profile | 包含服务 |
|---------|---------|
| `database` | MySQL, PostgreSQL, MongoDB |
| `cache` | Redis |
| `mq` | RabbitMQ |
| `kafka` | Kafka, Zookeeper |
| `search` | Elasticsearch |
| `storage` | MinIO |
| `web` | Nginx, Traefik |
| `discovery` | Consul, Etcd |
| `timeseries` | InfluxDB |
| `graph` | Neo4j |
| `mqtt` | EMQX |
| `tools` | 管理界面工具 |
| `full` | 所有服务 |

## 服务端口一览

### 数据库
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| MySQL | 8.0 | 3306 | phpMyAdmin: 8080 |
| MySQL | 5.7 | 3307 | phpMyAdmin: 8081 |
| PostgreSQL | 16 | 5432 | pgAdmin: 5050 |
| PostgreSQL | 14 | 5433 | - |
| MongoDB | 7 | 27017 | Mongo Express: 8081 |
| MongoDB | 6 | 27018 | - |
| ClickHouse | 24 | 8123 (HTTP), 9000 (Native) | - |

### 缓存
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Redis | 7 | 6379 | Redis Commander: 8081 |
| Redis | 6 | 6380 | - |
| Redis Cluster | - | 7001-7006 | - |
| Dragonfly | latest | 6379 | - |

### 消息队列
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| RabbitMQ | 3 | 5672 (AMQP) | Management: 15672 |
| Kafka | 3 | 9092 | Kafka UI: 9091 |
| NSQ | latest | 4150 (TCP) | Admin: 4171 |

### 搜索引擎
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Elasticsearch | 8 | 9200 | Kibana: 5601 |
| Elasticsearch | 7 | 9201 | Kibana: 5602 |

### 图数据库
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Neo4j | 5 | 7687 (Bolt) | HTTP: 7474 |
| ArangoDB | 3 | 8529 | 内置: 8529 |
| OrientDB | 3 | 2424 | HTTP: 2480 |

### 时序数据库
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| InfluxDB | 2 | 8086 | 内置: 8086 |

### 对象存储
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| MinIO | latest | 9000 (API) | Console: 9001 |

### Web服务器/反向代理
| 服务 | 版本 | HTTP | HTTPS | 管理界面 |
|------|------|------|-------|----------|
| Nginx | alpine | 80 | 443 | - |
| Traefik | 3 | 80 | 443 | Dashboard: 8080 |
| Caddy | 2 | 80 | 443 | - |
| OpenResty | alpine | 80 | 443 | - |

### API网关
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Kong | 3 | 8000 (Proxy), 8001 (Admin) | Konga: 1337 |

### 服务发现
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Consul | 1 | 8500 | UI: 8500 |
| Etcd | 3 | 2379 | etcdkeeper: 8080 |

### MQTT
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| EMQX | 5 | 1883 (MQTT), 8083 (WS) | Dashboard: 18083 |

### Git服务
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Gitea | 1 | 3000 | 内置: 3000 |
| GitLab | CE | 8929 | 内置: 8929 |

### 其他
| 服务 | 版本 | 端口 | 说明 |
|------|------|------|------|
| Imgproxy | 3 | 8080 | 图片处理 |
| NPS | latest | 8080, 8024 | 内网穿透 |
| RustFS | latest | 9000 | 文件系统 |

## 默认账号密码

> **警告**: 生产环境请务必修改默认密码！

| 服务 | 用户名 | 密码 |
|------|--------|------|
| MySQL root | root | root123456 |
| PostgreSQL | postgres | postgres123456 |
| MongoDB | root | root123456 |
| RabbitMQ | admin | admin123456 |
| MinIO | minioadmin | minioadmin123456 |
| Neo4j | neo4j | neo4j123456 |
| EMQX | admin | admin123456 |
| Gitea | (首次访问设置) | - |
| GitLab | root | gitlab123456 |
| Konga | admin | admin123456 |

## 常用命令

```bash
# 查看所有服务状态
docker-compose ps

# 查看服务日志
docker-compose logs -f <service>

# 重启服务
docker-compose restart <service>

# 进入容器
docker-compose exec <service> sh

# 拉取所有镜像
docker-compose pull

# 清理停止的容器
docker-compose rm -f

# 查看资源使用
docker stats

# 单独服务目录
cd redis/redis7
docker-compose up -d
docker-compose logs -f
docker-compose down
```

## 生产环境建议

1. **修改默认密码**: 复制 `.env.example` 为 `.env` 并修改所有密码
2. **配置持久化**: 确保数据卷正确映射到持久存储
3. **资源限制**: 根据需要添加 `deploy.resources` 配置
4. **安全配置**:
   - 启用 TLS/SSL
   - 配置防火墙规则
   - 使用 Docker secrets 管理敏感信息
5. **备份策略**: 定期备份重要数据卷
6. **监控告警**: 集成 Prometheus + Grafana 监控

## 资源限制示例

```yaml
services:
  mysql:
    # ... 其他配置
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 2G
        reservations:
          cpus: '0.5'
          memory: 512M
```

## 常见问题

### 端口冲突
如果默认端口被占用，修改 `.env` 文件中的端口配置，或直接进入单独服务目录修改。

### 内存不足
Elasticsearch 等服务需要较多内存，可通过 `ES_JAVA_OPTS` 调整 JVM 堆大小。

### Windows/Mac 文件权限
确保 Docker 有访问挂载目录的权限。

## License

MIT
