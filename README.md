# Awesome Docker Compose

一套完整的 Docker Compose 模板集合，包含常用服务的开箱即用配置。

## 快速开始

```bash
# 复制环境变量文件
cp .env.example .env

# 编辑环境变量（可选）
vim .env

# 启动所有服务（使用 full profile）
docker-compose --profile full up -d

# 启动特定服务组
docker-compose --profile database up -d
docker-compose --profile cache up -d
docker-compose --profile mq up -d

# 启动单个服务
cd mysql && docker-compose up -d
```

## 目录结构

```
awesome-docker-compose/
├── docker-compose.yml          # 集成 Starter（主文件）
├── .env.example                # 环境变量示例
├── README.md                   # 文档
│
├── mysql/                      # MySQL 8.0 + phpMyAdmin
├── postgres/                   # PostgreSQL 16 + pgAdmin
├── mongo/                      # MongoDB 7 + Mongo Express
├── redis/                      # Redis 7 + Redis Commander
├── rabbitmq/                   # RabbitMQ 3 + Management UI
├── kafka/                      # Kafka 3.6 + Zookeeper + Kafka UI
├── elasticsearch/              # Elasticsearch 8 + Kibana
├── minio/                      # MinIO 对象存储
├── nginx/                      # Nginx Web服务器
├── traefik/                    # Traefik 反向代理
├── caddy/                      # Caddy Web服务器
├── consul/                     # Consul 服务发现
├── etcd/                       # Etcd 键值存储
├── neo4j/                      # Neo4j 图数据库
├── influxdb/                   # InfluxDB 2 + Telegraf
├── clickhouse/                 # ClickHouse 列式数据库
├── arangodb/                   # ArangoDB 图数据库
├── orientdb/                   # OrientDB 图数据库
├── dragonfly/                  # Dragonfly Redis替代
├── emqx/                       # EMQX MQTT Broker
├── nsq/                        # NSQ 消息队列
├── kong/                       # Kong API网关
├── openresty/                  # OpenResty
├── gitea/                      # Gitea Git服务
├── gitlab/                     # GitLab CE
├── imgproxy/                   # Imgproxy 图片处理
├── nps/                        # NPS 内网穿透
├── rustfs/                     # RustFS 文件系统
├── debian/                     # Debian 容器
└── ubuntu/                     # Ubuntu 容器
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
| `tools` | Adminer, Redis Commander, Kafka UI, Mongo Express, Kibana |
| `full` | 所有服务 |

## 单独服务使用

每个服务目录都有独立的 `docker-compose.yml`：

```bash
# 进入服务目录
cd mysql

# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down

# 停止并删除数据卷
docker-compose down -v
```

## 服务端口一览

### 数据库
| 服务 | 端口 | 管理界面 |
|------|------|----------|
| MySQL | 3306 | phpMyAdmin: 8080 |
| PostgreSQL | 5432 | pgAdmin: 5050 |
| MongoDB | 27017 | Mongo Express: 8081 |
| Redis | 6379 | Redis Commander: 8081 |

### 消息队列
| 服务 | 端口 | 管理界面 |
|------|------|----------|
| RabbitMQ | 5672 (AMQP) | Management: 15672 |
| Kafka | 9092 | Kafka UI: 8080 |
| NSQ | 4150 (TCP) | Admin: 4171 |

### 搜索 & 存储
| 服务 | 端口 | 管理界面 |
|------|------|----------|
| Elasticsearch | 9200 | Kibana: 5601 |
| MinIO | 9000 (API) | Console: 9001 |

### Web服务器
| 服务 | HTTP | HTTPS | 管理界面 |
|------|------|-------|----------|
| Nginx | 80 | 443 | - |
| Traefik | 80 | 443 | Dashboard: 8080 |
| Caddy | 80 | 443 | - |

### 服务发现
| 服务 | 端口 | 管理界面 |
|------|------|----------|
| Consul | 8500 | UI: 8500 |
| Etcd | 2379 | etcdkeeper: 8080 |

### 其他
| 服务 | 端口 | 管理界面 |
|------|------|----------|
| Neo4j | 7687 (Bolt) | HTTP: 7474 |
| InfluxDB | 8086 | 内置: 8086 |
| ClickHouse | 8123 (HTTP), 9000 (Native) | - |
| ArangoDB | 8529 | 内置: 8529 |
| EMQX | 1883 (MQTT), 8083 (WS) | Dashboard: 18083 |
| Gitea | 3000 | 内置: 3000 |
| GitLab | 8929 | 内置: 8929 |

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
| Consul | - | - |
| Gitea | (首次访问设置) | - |
| GitLab | root | gitlab123456 |

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

# 构建镜像（如果有 build 配置）
docker-compose build

# 清理停止的容器
docker-compose rm -f

# 查看资源使用
docker stats
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
如果默认端口被占用，修改 `.env` 文件中的端口配置。

### 内存不足
Elasticsearch 等服务需要较多内存，可通过 `ES_JAVA_OPTS` 调整 JVM 堆大小。

### Windows/Mac 文件权限
确保 Docker 有访问挂载目录的权限。

## License

MIT
