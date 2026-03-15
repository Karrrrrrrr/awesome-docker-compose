# Elasticsearch

Elasticsearch 是一个分布式搜索和分析引擎。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `es8/` | 8.x + Kibana | 最新版本 |
| `es7/` | 7.x + Kibana | 稳定版本 |
| `elk/` | 8.x ELK Stack | 完整全家桶 |

## ELK Stack 组件

| 组件 | 端口 | 说明 |
|------|------|------|
| Elasticsearch | 9200, 9300 | 搜索引擎 |
| Logstash | 5044, 9600, 8080 | 数据管道 |
| Kibana | 5601 | 可视化界面 |
| Filebeat | - | 日志收集 |
| Metricbeat | - | 指标收集 |

## 快速启动

```bash
# ES 8 + Kibana
cd es8 && docker-compose up -d

# ES 7 + Kibana
cd es7 && docker-compose up -d

# 完整ELK
cd elk && docker-compose up -d
```

## 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| ES_PORT | 9200 | HTTP端口 |
| KIBANA_PORT | 5601 | Kibana端口 |
| ELASTIC_PASSWORD | elastic123456 | elastic用户密码 |
| LOGSTASH_BEATS_PORT | 5044 | Beats输入端口 |

## 健康检查

```bash
# ES健康
curl -u elastic:elastic123456 http://localhost:9200/_cluster/health

# Kibana状态
curl http://localhost:5601/api/status

# Logstash状态
curl http://localhost:9600/_node/stats
```

## 常用操作

```bash
# 查看索引
curl -u elastic:elastic123456 http://localhost:9200/_cat/indices?v

# 查看节点
curl -u elastic:elastic123456 http://localhost:9200/_cat/nodes?v

# 创建索引
curl -X PUT "http://elastic:elastic123456@localhost:9200/my-index"

# 删除索引
curl -X DELETE "http://elastic:elastic123456@localhost:9200/my-index"
```

## Kibana 访问

访问 http://localhost:5601

默认账号:
- 用户: elastic
- 密码: elastic123456

## 持久化

- ES数据: `elasticsearch_data`
- Logstash配置: `./logstash/`
- Kibana配置: `./kibana/`
