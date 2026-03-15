# Neo4j

Neo4j 是一个高性能的图数据库。

## 版本

- `neo4j5/`: Neo4j 5 Community

## 快速启动

```bash
cd neo4j5 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 7474 | HTTP Browser |
| 7473 | HTTPS Browser |
| 7687 | Bolt协议 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| NEO4J_AUTH | neo4j/neo4j123456 |
| NEO4J_server_memory_pagecache_size | 256M |
| NEO4J_server_memory_heap_initial__size | 512M |
| NEO4J_server_memory_heap_max__size | 512M |

## Neo4j Browser

访问 http://localhost:7474

登录: neo4j / neo4j123456

## Cypher查询语言

```cypher
// 创建节点
CREATE (p:Person {name: 'Alice', age: 30})
CREATE (p:Person {name: 'Bob', age: 25})

// 创建关系
MATCH (a:Person {name: 'Alice'})
MATCH (b:Person {name: 'Bob'})
CREATE (a)-[:KNOWS {since: 2020}]->(b)

// 查询
MATCH (p:Person)-[:KNOWS]->(friend)
RETURN p.name, friend.name

// 查询所有Person
MATCH (p:Person)
RETURN p.name, p.age

// 更新
MATCH (p:Person {name: 'Alice'})
SET p.age = 31

// 删除
MATCH (p:Person {name: 'Bob'})
DELETE p

// 删除关系
MATCH (a:Person {name: 'Alice'})-[r:KNOWS]->()
DELETE r
```

## 常用操作

```bash
# 进入容器
docker exec -it neo4j5 bash

# 备份数据库
docker exec neo4j5 neo4j-admin database dump neo4j --to-path=/backups

# 恢复数据库
docker exec neo4j5 neo4j-admin database load neo4j --from-path=/backups

# 查看状态
docker exec neo4j5 neo4j status

# 重启
docker exec neo4j5 neo4j restart
```

## Python驱动

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver("bolt://localhost:7687",
                              auth=("neo4j", "neo4j123456"))

def add_person(tx, name, age):
    tx.run("CREATE (p:Person {name: $name, age: $age})",
           name=name, age=age)

with driver.session() as session:
    session.execute_write(add_person, "Alice", 30)

driver.close()
```

## 数据导入

### CSV导入

```cypher
LOAD CSV WITH HEADERS FROM 'file:///people.csv' AS row
CREATE (p:Person {name: row.name, age: toInteger(row.age)})
```

## 持久化

- neo4j_data: 数据
- neo4j_logs: 日志
- neo4j_import: 导入文件
- neo4j_plugins: 插件
