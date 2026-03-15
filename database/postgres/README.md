# PostgreSQL

PostgreSQL 是一个功能强大的开源对象关系数据库系统。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `postgres16/` | 16 | 最新版本，推荐使用 |
| `postgres14/` | 14 | LTS版本 |
| `postgres16-bitnami/` | 16 (Bitnami) | 生产级镜像 |

## 快速启动

```bash
cd postgres16 && docker-compose up -d
```

## 端口

| 服务 | 端口 |
|------|------|
| PostgreSQL | 5432 |
| pgAdmin | 5050 |

## 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| POSTGRES_USER | postgres | 超级用户 |
| POSTGRES_PASSWORD | postgres123456 | 密码 |
| POSTGRES_DB | app | 默认数据库 |
| PGADMIN_EMAIL | admin@admin.com | pgAdmin邮箱 |
| PGADMIN_PASSWORD | admin123456 | pgAdmin密码 |

## 连接示例

```bash
# 命令行
psql -h 127.0.0.1 -U postgres -d app

# Docker
docker exec -it postgres16 psql -U postgres
```

## 常用操作

```bash
# 备份
docker exec postgres16 pg_dump -U postgres app > backup.sql

# 恢复
docker exec -i postgres16 psql -U postgres app < backup.sql

# 查看连接
docker exec postgres16 psql -U postgres -c "SELECT * FROM pg_stat_activity;"
```

## 持久化

- 数据: `postgres_data` volume
- pgAdmin: `pgadmin_data` volume
