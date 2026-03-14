# OrientDB

OrientDB 是一个多模型数据库，支持文档、图、键值和对象模型。

## 快速启动

```bash
cd orientdb3 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 2424 | 二进制协议 |
| 2480 | HTTP/Studio |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| ORIENTDB_ROOT_PASSWORD | root123456 |

## Web界面 (Studio)

访问 http://localhost:2480

登录: root / root123456

## SQL示例

### 数据库操作

```sql
-- 创建数据库
CREATE DATABASE mydb

-- 切换数据库
USE mydb

-- 创建类 (表)
CREATE CLASS Person

-- 创建属性
CREATE PROPERTY Person.name STRING
CREATE PROPERTY Person.age INTEGER

-- 添加约束
ALTER PROPERTY Person.name MANDATORY true
```

### 文档操作

```sql
-- 插入文档
INSERT INTO Person SET name = 'Alice', age = 30

-- 查询
SELECT FROM Person WHERE age > 25

-- 更新
UPDATE Person SET age = 31 WHERE name = 'Alice'

-- 删除
DELETE FROM Person WHERE name = 'Bob'
```

### 图操作

```sql
-- 创建顶点类
CREATE CLASS Person EXTENDS V

-- 创建边类
CREATE CLASS Knows EXTENDS E

-- 创建顶点
CREATE VERTEX Person SET name = 'Alice'
CREATE VERTEX Person SET name = 'Bob'

-- 创建边
CREATE EDGE Knows FROM (SELECT FROM Person WHERE name = 'Alice')
TO (SELECT FROM Person WHERE name = 'Bob')
SET since = '2020'

-- 遍历
SELECT expand(out('Knows')) FROM Person WHERE name = 'Alice'

-- 深度遍历
SELECT expand(out('Knows').out('Knows')) FROM Person WHERE name = 'Alice'

-- 最短路径
SELECT shortestPath($from, $to)
LET $from = (SELECT FROM Person WHERE name = 'Alice'),
    $to = (SELECT FROM Person WHERE name = 'Bob')
```

### 索引

```sql
-- 创建索引
CREATE INDEX Person.name ON Person (name) UNIQUE

-- 创建全文索引
CREATE INDEX Person.bio FULLTEXT ENGINE LUCENE ON Person (bio)

-- 搜索
SELECT FROM Person WHERE bio LUCENE "developer"
```

### 函数

```sql
-- 聚合
SELECT name, count(*) FROM Person GROUP BY name

-- 数学函数
SELECT avg(age), max(age), min(age) FROM Person

-- 字符串函数
SELECT name, upper(name), lower(name) FROM Person
```

## REST API

```bash
# 查询
curl -u root:root123456 \
  "http://localhost:2480/query/mydb/sql/SELECT FROM Person"

# 命令
curl -u root:root123456 \
  -X POST \
  "http://localhost:2480/command/mydb/sql" \
  -d "INSERT INTO Person SET name='Test'"
```

## 备份

```bash
# 备份数据库
docker exec orientdb3 /orientdb/bin/console.sh "CONNECT plocal:/orientdb/databases/mydb root root123456; BACKUP DATABASE /backup/mydb.zip"

# 恢复数据库
docker exec orientdb3 /orientdb/bin/console.sh "CREATE DATABASE mydb; RESTORE DATABASE /backup/mydb.zip"
```

## 常用命令

```bash
# 连接控制台
docker exec -it orientdb3 /orientdb/bin/console.sh

# 查看数据库
docker exec orientdb3 /orientdb/bin/console.sh "LIST DATABASES"

# 查看类
docker exec orientdb3 /orientdb/bin/console.sh "CONNECT plocal:/orientdb/databases/mydb root root123456; LIST CLASSES"
```

## 持久化

- orientdb_data: 数据
- orientdb_backup: 备份
