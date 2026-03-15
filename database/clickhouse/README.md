# ClickHouse

ClickHouse 是一个用于联机分析(OLAP)的列式数据库管理系统。

## 版本

- `clickhouse24/`: ClickHouse 24

## 快速启动

```bash
cd clickhouse24 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8123 | HTTP接口 |
| 9000 | Native接口 |
| 9004 | MySQL兼容接口 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| CLICKHOUSE_DB | default |
| CLICKHOUSE_USER | default |
| CLICKHOUSE_PASSWORD | clickhouse123456 |

## 连接

### HTTP

```bash
curl 'http://localhost:8123/?user=default&password=clickhouse123456' \
  -d 'SELECT 1'
```

### 命令行

```bash
docker exec -it clickhouse24 clickhouse-client \
  --user default --password clickhouse123456
```

### Python

```python
from clickhouse_driver import Client

client = Client('localhost', user='default', password='clickhouse123456')
result = client.execute('SELECT 1')
```

## SQL示例

```sql
-- 创建表
CREATE TABLE events (
    event_date Date,
    event_time DateTime,
    user_id UInt64,
    event_type String
) ENGINE = MergeTree()
PARTITION BY toYYYYMM(event_date)
ORDER BY (event_date, event_time);

-- 插入数据
INSERT INTO events VALUES
    ('2024-01-01', '2024-01-01 12:00:00', 1, 'click'),
    ('2024-01-01', '2024-01-01 12:01:00', 2, 'view'),
    ('2024-01-01', '2024-01-01 12:02:00', 1, 'purchase');

-- 查询
SELECT
    event_type,
    count() as count
FROM events
GROUP BY event_type;

-- 物化视图
CREATE MATERIALIZED VIEW events_daily
ENGINE = SummingMergeTree()
ORDER BY (event_date, event_type)
AS SELECT
    event_date,
    event_type,
    count() as count
FROM events
GROUP BY event_date, event_type;
```

## 数据导入

```bash
# CSV导入
docker exec -i clickhouse24 clickhouse-client \
  --query="INSERT INTO events FORMAT CSV" < data.csv

# JSON导入
docker exec -i clickhouse24 clickhouse-client \
  --query="INSERT INTO events FORMAT JSONEachRow" < data.json
```

## 配置

- `./config.d/`: 额外配置
- `./users.d/`: 用户配置

## 持久化

- clickhouse_data: 数据
- clickhouse_logs: 日志
