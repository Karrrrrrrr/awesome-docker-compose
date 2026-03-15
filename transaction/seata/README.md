# Seata

Seata 是一款开源的分布式事务解决方案，提供高性能和简单易用的分布式事务服务。由阿里巴巴开源。

## 快速启动

```bash
cd seata && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8091 | Seata Server (TC) 服务端口 |
| 7091 | Console 控制台端口 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| SEATA_PORT | 8091 |
| SEATA_CONSOLE_PORT | 7091 |

## 访问界面

启动后访问 http://localhost:7091 查看 Seata Console。

默认账号密码: seata / seata

## 事务模式

Seata 支持以下分布式事务模式：

### 1. AT 模式（推荐）
- 最简单的分布式事务模式
- 无侵入式，业务代码零修改
- 自动补偿回滚

### 2. TCC 模式
- 高性能
- 业务需要实现 Try/Confirm/Cancel 三个接口
- 适合强一致性要求高的场景

### 3. Saga 模式
- 长事务解决方案
- 适合业务流程复杂的场景
- 支持状态机编排

### 4. XA 模式
- 传统 XA 协议
- 强一致性
- 数据库需支持 XA 协议

## 使用示例

### Spring Cloud 集成

```yaml
# application.yml
seata:
  enabled: true
  application-id: my-application
  tx-service-group: my_tx_group
  service:
    vgroup-mapping:
      my_tx_group: default
  registry:
    type: file
  config:
    type: file
```

### 使用 @GlobalTransactional 注解

```java
@Service
public class OrderService {

    @GlobalTransactional
    public void createOrder(Order order) {
        // 创建订单
        orderMapper.insert(order);

        // 扣减库存
        storageService.deduct(order.getProductId(), order.getCount());

        // 扣减余额
        accountService.debit(order.getUserId(), order.getAmount());
    }
}
```

### 数据库表创建（AT 模式）

```sql
-- 每个业务数据库都需要创建 undo_log 表
CREATE TABLE `undo_log` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `branch_id` bigint NOT NULL,
  `xid` varchar(100) NOT NULL,
  `context` varchar(128) NOT NULL,
  `rollback_info` longblob NOT NULL,
  `log_status` int NOT NULL,
  `log_created` datetime NOT NULL,
  `log_modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

### Go 客户端

```go
import "github.com/seata/seata-go/pkg/client"

// 初始化 Seata 客户端
client.Init()

// 开启分布式事务
ctx := context.Background()
xid := client.Begin(ctx, "my_tx_group")

// 执行业务逻辑...

// 提交或回滚
client.Commit(ctx, xid)
// client.Rollback(ctx, xid)
```

## 存储模式

### File 模式（默认）
- 单机模式，数据存储在文件中
- 适合开发测试

### DB 模式
- 数据存储在数据库中
- 生产环境推荐

### Redis 模式
- 数据存储在 Redis 中
- 高性能场景

## 注册中心支持

- Nacos（推荐）
- Eureka
- Redis
- ZooKeeper
- Consul
- Etcd
- Sofa

## 配置中心支持

- Nacos（推荐）
- Apollo
- ZooKeeper
- Consul
- Etcd

## 与 DTM 对比

| 特性 | Seata | DTM |
|------|-------|-----|
| 开发语言 | Java | Go |
| AT 模式 | 支持 | 不支持 |
| TCC 模式 | 支持 | 支持 |
| Saga 模式 | 支持 | 支持 |
| XA 模式 | 支持 | 支持 |
| 多语言 | Java, Go, PHP, Python, Node.js | Go, Java, Python, PHP, Node.js |
| 性能 | 高 | 更高 |
| 学习曲线 | 较陡 | 平缓 |

## 健康检查

```bash
curl http://localhost:7091/health
```

## 相关链接

- 官网: https://seata.io/
- GitHub: https://github.com/seata/seata
- 文档: https://seata.io/zh-cn/docs/overview/what-is-seata.html
