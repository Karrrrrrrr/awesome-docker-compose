# Awesome Docker Compose

一套完整的 Docker Compose 模板集合，包含常用服务的开箱即用配置。

## 特点

- **版本化管理**: 每个服务按版本独立目录，方便选择特定版本
- **多版本支持**: 热门服务提供多个版本选择
- **多镜像源**: 提供官方镜像、Bitnami、LinuxServer.io等多种选择
- **完整配置**: 包含配置文件、健康检查、持久化存储
- **管理界面**: 大部分服务附带Web管理界面

## 镜像源说明

| 镜像源 | 特点 | 目录后缀 |
|--------|------|----------|
| **官方镜像** | Docker Hub 官方维护 | 默认 |
| **Bitnami** | 更安全、更精简、生产级 | `-bitnami` |
| **LinuxServer.io** | 社区维护、支持PUID/PGID | `linuxserver/` |

## 目录结构

```
awesome-docker-compose/
├── docker-compose.yml              # 主集成文件
├── .env.example                    # 环境变量模板
├── README.md                       # 文档
│
├── mysql/
│   ├── mysql8/docker-compose.yml       # MySQL 8.0 + phpMyAdmin
│   ├── mysql5/docker-compose.yml       # MySQL 5.7 + phpMyAdmin
│   └── mysql8-bitnami/docker-compose.yml  # MySQL 8.0 (Bitnami)
│
├── postgres/
│   ├── postgres16/docker-compose.yml   # PostgreSQL 16 + pgAdmin
│   ├── postgres14/docker-compose.yml   # PostgreSQL 14
│   └── postgres16-bitnami/docker-compose.yml  # PostgreSQL 16 (Bitnami)
│
├── mongo/
│   ├── mongo7/docker-compose.yml       # MongoDB 7 + Mongo Express
│   ├── mongo6/docker-compose.yml       # MongoDB 6
│   └── mongo7-bitnami/docker-compose.yml  # MongoDB 7 (Bitnami)
│
├── redis/
│   ├── redis7/docker-compose.yml       # Redis 7 + Redis Commander
│   ├── redis6/docker-compose.yml       # Redis 6
│   ├── redis7-bitnami/docker-compose.yml  # Redis 7 (Bitnami)
│   ├── redis-cluster/docker-compose.yml   # Redis Cluster (6节点)
│   └── redis-sentinel/docker-compose.yml  # Redis Sentinel (高可用)
│
├── rabbitmq/
│   ├── rabbitmq4/docker-compose.yml    # RabbitMQ 4 + Management UI
│   ├── rabbitmq3/docker-compose.yml    # RabbitMQ 3 + Management UI
│   └── rabbitmq3-bitnami/docker-compose.yml  # RabbitMQ 3 (Bitnami)
│
├── kafka/
│   ├── kafka3/docker-compose.yml       # Kafka 3 + Zookeeper + Kafka UI
│   ├── kafka3-bitnami/docker-compose.yml   # Kafka 3 (Bitnami)
│   └── kafka-kraft/docker-compose.yml  # Kafka KRaft模式 (无Zookeeper)
│
├── elasticsearch/
│   ├── elk/docker-compose.yml      # ELK全家桶 (ES + Logstash + Kibana + Beats)
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
│   ├── arangodb3/docker-compose.yml  # ArangoDB 3
│   └── arangodb2/docker-compose.yml  # ArangoDB 2
│
├── orientdb/
│   └── orientdb3/docker-compose.yml  # OrientDB 3
│
├── influxdb/
│   ├── influxdb2/docker-compose.yml  # InfluxDB 2 + Telegraf
│   └── influxdb1/docker-compose.yml  # InfluxDB 1.8 + Chronograf + Kapacitor
│
├── minio/
│   └── minio/docker-compose.yml    # MinIO 对象存储
│
├── nginx/
│   └── nginx/docker-compose.yml    # Nginx
│
├── traefik/
│   ├── traefik3/docker-compose.yml # Traefik 3
│   └── traefik2/docker-compose.yml # Traefik 2
│
├── caddy/
│   └── caddy2/docker-compose.yml   # Caddy 2
│
├── consul/
│   ├── consul1/docker-compose.yml  # Consul 1
│   └── consul-bitnami/docker-compose.yml  # Consul (Bitnami)
│
├── etcd/
│   ├── etcd3/docker-compose.yml    # Etcd 3 + etcdkeeper
│   └── etcd2/docker-compose.yml    # Etcd 2
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
│   ├── gitea1/docker-compose.yml   # Gitea 1
│   └── gitea1-bitnami/docker-compose.yml  # Gitea 1 (Bitnami)
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
├── ubuntu/
│   └── ubuntu22/docker-compose.yml # Ubuntu 22.04
│
└── linuxserver/                    # LinuxServer.io 镜像
    ├── nginx/docker-compose.yml        # Nginx
    ├── redis/docker-compose.yml        # Redis
    ├── code-server/docker-compose.yml  # VS Code Web
    ├── syncthing/docker-compose.yml    # 文件同步
    ├── nextcloud/docker-compose.yml    # 私有云存储
    ├── portainer/docker-compose.yml    # Docker管理
    ├── uptime-kuma/docker-compose.yml  # 服务监控
    ├── jellyfin/docker-compose.yml     # 媒体服务器
    ├── filebrowser/docker-compose.yml  # 文件管理
    ├── homeassistant/docker-compose.yml # 智能家居
    ├── n8n/docker-compose.yml          # 工作流自动化
    └── vaultwarden/docker-compose.yml  # 密码管理器
│
├── monitoring/                   # 监控服务
│   ├── prometheus-grafana/        # Prometheus + Grafana
│   ├── dozzle/                    # Docker日志查看
│   ├── loki/                      # 日志聚合
│   └── promtail/                  # 日志收集代理
│
├── ci-cd/                        # CI/CD
│   ├── jenkins/                   # Jenkins
│   └── drone/                     # Drone CI
│
├── utility/                      # 实用工具
│   ├── watchtower/               # 自动更新容器
│   ├── it-tools/                 # IT工具集
│   ├── memos/                    # 笔记
│   ├── outline/                  # 知识库Wiki
│   ├── trilium/                  # 知识管理
│   ├── traefik-whoami/            # 测试服务
│   └── httpbin/                   # HTTP测试
│
├── ai/                           # AI服务
│   ├── ollama/                   # 本地LLM
│   └── stable-diffusion/          # AI图像生成
│
├── vpn/                          # VPN/内网穿透
│   ├── wireguard/                # Wireguard VPN
│   └── frp/                      # FRP内网穿透
│
├── messaging/                    # 消息推送
│   ├── gotify/                   # Gotify
│   └── ntfy/                     # ntfy
│
├── database/                     # 数据库工具
│   ├── adminer/                  # Adminer
│   ├── cloudbeaver/              # CloudBeaver
│   └── pgweb/                    # pgweb
│
├── design/                       # 设计工具
│   ├── penpot/                   # 设计协作
│   └── excalidraw/               # 白板绘图
│
└── media/                       # 媒体服务
    ├── photoprism/               # 照片管理
    ├── immich/                   # 照片视频管理
    └── navidrome/                # 视频监控NVR
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
| RabbitMQ | 4 | 5672 (AMQP) | Management: 15672 |
| RabbitMQ | 3 | 5673 (AMQP) | Management: 15673 |
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
| InfluxDB | 1.8 | 8086 | Chronograf: 8888 |

### 搜索引擎/日志
| 服务 | 版本 | 端口 | 管理界面 |
|------|------|------|----------|
| Elasticsearch | 8 | 9200 | Kibana: 5601 |
| Elasticsearch | 7 | 9201 | Kibana: 5602 |
| ELK Stack | 8 | ES:9200, LS:5044, KB:5601 | Kibana: 5601 |

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

### LinuxServer.io / Home Lab
| 服务 | 端口 | 说明 |
|------|------|------|
| Code Server | 8443 | VS Code Web版 |
| Syncthing | 8384 | 文件同步 |
| Nextcloud | 444 | 私有云存储 |
| Portainer | 9000 | Docker管理界面 |
| Uptime Kuma | 3001 | 服务监控 |
| Jellyfin | 8096 | 媒体服务器 |
| FileBrowser | 8085 | Web文件管理 |
| Home Assistant | 8123 | 智能家居 |
| n8n | 5678 | 工作流自动化 |
| Vaultwarden | 8123 | 密码管理器 |

### 监控/可观测性
| 服务 | 端口 | 说明 |
|------|------|------|
| Prometheus | 9090 | 监控系统 |
| Grafana | 3000 | 可视化面板 |
| Dozzle | 9999 | Docker日志查看 |
| Loki | 3100 | 日志聚合 |
| Promtail | 9080 | 日志收集 |

### CI/CD
| 服务 | 端口 | 说明 |
|------|------|------|
| Jenkins | 8080 | CI/CD服务器 |
| Drone CI | 80 | 轻量级CI/CD |

### AI/LLM
| 服务 | 端口 | 说明 |
|------|------|------|
| Ollama | 11434 | 本地LLM运行 |
| Open WebUI | 3000 | LLM Web界面 |
| Stable Diffusion | 7860 | AI图像生成 |

### VPN/内网穿透
| 服务 | 端口 | 说明 |
|------|------|------|
| Wireguard | 51820 | VPN服务 |
| FRP | 7000, 7500 | 内网穿透 |

### 消息推送
| 服务 | 端口 | 说明 |
|------|------|------|
| Gotify | 8080 | 消息推送 |
| ntfy | 8080 | 推送通知 |

### 数据库工具
| 服务 | 端口 | 说明 |
|------|------|------|
| Adminer | 8080 | 轻量级数据库管理 |
| CloudBeaver | 8978 | 企业级数据库管理 |
| pgweb | 8081 | PostgreSQL Web管理 |

### 设计工具
| 服务 | 端口 | 说明 |
|------|------|------|
| Penpot | 9001 | 设计协作平台 |
| Excalidraw | 8080 | 白板绘图 |

### 媒体服务
| 服务 | 端口 | 说明 |
|------|------|------|
| PhotoPrism | 2342 | 照片管理 |
| Immich | 2283 | 照片视频管理 |
| Navirome | 3443 | 视频监控NVR |

### 实用工具
| 服务 | 端口 | 说明 |
|------|------|------|
| Watchtower | - | 自动更新容器 |
| IT-Tools | 8080 | IT工具集 |
| Memos | 5230 | 备忘录 |
| Outline | 3000 | 知识库Wiki |
| Trilium | 8080 | 知识管理 |
| Hoppscotch | 3000 | API测试 |
| HTTPBin | 8080 | HTTP测试 |
| Whoami | 8080 | 测试服务 |

### 其他
| 服务 | 版本 | 端口 | 说明 |
|------|------|------|------|
| Imgproxy | 3 | 8080 | 图片处理 |
| NPS | latest | 8080, 8024 | 内网穿透 |
| RustFS | latest | 9000 | 文件系统 |

## 镜像源说明

| 镜像源 | 特点 | 使用场景 |
|--------|------|----------|
| **官方镜像** | Docker Hub 官方维护 | 默认选择 |
| **Bitnami** | 更安全、更精简、生产级 | 生产环境 |
| **LinuxServer.io** | 社区维护、支持PUID/PGID | 家庭服务器 |

### Bitnami 镜像优势
- 镜像更精简，安全性更高
- 定期更新，包含安全补丁
- 提供完整的环境变量配置
- 生产环境推荐

### LinuxServer.io 镜像优势
- 统一的 PUID/PGID 权限管理
- 适合 NAS 和家庭服务器
- 活跃的社区支持

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
