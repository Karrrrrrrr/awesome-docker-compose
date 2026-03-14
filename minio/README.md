# MinIO

MinIO 是高性能的对象存储服务，兼容 Amazon S3 API。

## 快速启动

```bash
cd minio && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 9000 | API端口 (S3兼容) |
| 9001 | Console管理界面 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| MINIO_ROOT_USER | minioadmin |
| MINIO_ROOT_PASSWORD | minioadmin123456 |
| MINIO_API_PORT | 9000 |
| MINIO_CONSOLE_PORT | 9001 |

## 访问

- API: http://localhost:9000
- Console: http://localhost:9001

## 默认账号

| 用户 | 密码 |
|------|------|
| minioadmin | minioadmin123456 |

## 客户端工具

### mc (MinIO Client)

```bash
# 配置别名
mc alias set myminio http://localhost:9000 minioadmin minioadmin123456

# 创建bucket
mc mb myminio/mybucket

# 上传文件
mc cp myfile.txt myminio/mybucket/

# 下载文件
mc cp myminio/mybucket/myfile.txt ./

# 列出文件
mc ls myminio/mybucket/

# 设置公开访问
mc anonymous set download myminio/public
```

### AWS CLI

```bash
# 配置
aws configure --profile minio
# Access Key: minioadmin
# Secret Key: minioadmin123456
# Region: us-east-1

# 使用
aws --endpoint-url http://localhost:9000 s3 ls --profile minio
```

### SDK示例

```python
# Python (boto3)
import boto3

s3 = boto3.client('s3',
    endpoint_url='http://localhost:9000',
    aws_access_key_id='minioadmin',
    aws_secret_access_key='minioadmin123456'
)

s3.list_buckets()
```

```javascript
// Node.js
const Minio = require('minio')

const minioClient = new Minio.Client({
    endPoint: 'localhost',
    port: 9000,
    useSSL: false,
    accessKey: 'minioadmin',
    secretKey: 'minioadmin123456'
})
```

## 初始化容器

启动时会自动创建以下bucket:
- public: 公开访问
- private: 私有访问

## 健康检查

```bash
curl -f http://localhost:9000/minio/health/live
```

## 持久化

数据存储在 `minio_data` volume 中。
