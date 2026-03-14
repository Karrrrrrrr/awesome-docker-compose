# RustFS

RustFS 是一个用 Rust 编写的分布式文件系统。

## 快速启动

```bash
cd rustfs && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 9000 | API端口 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| RUSTFS_ACCESS_KEY | rustfsadmin |
| RUSTFS_SECRET_KEY | rustfsadmin123456 |
| RUSTFS_PORT | 9000 |
| RUSTFS_REGION | us-east-1 |

## S3兼容API

RustFS 兼容 Amazon S3 API。

### AWS CLI

```bash
# 配置
aws configure --profile rustfs
# Access Key: rustfsadmin
# Secret Key: rustfsadmin123456
# Region: us-east-1

# 创建bucket
aws --endpoint-url http://localhost:9000 s3 mb s3://mybucket --profile rustfs

# 上传文件
aws --endpoint-url http://localhost:9000 s3 cp file.txt s3://mybucket/ --profile rustfs

# 下载文件
aws --endpoint-url http://localhost:9000 s3 cp s3://mybucket/file.txt ./ --profile rustfs

# 列出文件
aws --endpoint-url http://localhost:9000 s3 ls s3://mybucket/ --profile rustfs

# 删除文件
aws --endpoint-url http://localhost:9000 s3 rm s3://mybucket/file.txt --profile rustfs
```

### Python (boto3)

```python
import boto3

s3 = boto3.client('s3',
    endpoint_url='http://localhost:9000',
    aws_access_key_id='rustfsadmin',
    aws_secret_access_key='rustfsadmin123456',
    region_name='us-east-1'
)

# 创建bucket
s3.create_bucket(Bucket='mybucket')

# 上传文件
s3.upload_file('local.txt', 'mybucket', 'remote.txt')

# 下载文件
s3.download_file('mybucket', 'remote.txt', 'downloaded.txt')

# 列出对象
response = s3.list_objects_v2(Bucket='mybucket')
for obj in response.get('Contents', []):
    print(obj['Key'])

# 删除对象
s3.delete_object(Bucket='mybucket', Key='remote.txt')
```

### Node.js

```javascript
const AWS = require('aws-sdk')

const s3 = new AWS.S3({
    endpoint: 'http://localhost:9000',
    accessKeyId: 'rustfsadmin',
    secretAccessKey: 'rustfsadmin123456',
    region: 'us-east-1',
    s3ForcePathStyle: true
})

// 创建bucket
await s3.createBucket({ Bucket: 'mybucket' }).promise()

// 上传
await s3.upload({
    Bucket: 'mybucket',
    Key: 'test.txt',
    Body: 'Hello World'
}).promise()

// 下载
const data = await s3.getObject({
    Bucket: 'mybucket',
    Key: 'test.txt'
}).promise()

console.log(data.Body.toString())
```

### curl

```bash
# 创建bucket
curl -X PUT http://localhost:9000/mybucket

# 上传对象
curl -X PUT -T file.txt http://localhost:9000/mybucket/file.txt

# 下载对象
curl -o download.txt http://localhost:9000/mybucket/file.txt

# 删除对象
curl -X DELETE http://localhost:9000/mybucket/file.txt
```

## 健康检查

```bash
curl http://localhost:9000/health
```

## 特点

- 高性能 (Rust实现)
- S3兼容API
- 轻量级部署
- 分布式架构

## 持久化

- rustfs_data: 数据存储
