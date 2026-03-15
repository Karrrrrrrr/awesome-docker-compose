# Imgproxy

Imgproxy 是一个快速安全的图片处理服务。

## 版本

- `imgproxy3/`: Imgproxy 3.x

## 快速启动

```bash
cd imgproxy3 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8080 | API端口 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| IMGPROXY_SECRET | imgproxy-secret |
| IMGPROXY_ALLOW_ORIGIN | * |

## 使用方式

### 基本URL格式

```
http://localhost:8080/%signature/%resizing_type/%width/%height/%gravity/%enlarge/%base64_url
```

### 无签名模式 (开发用)

```
http://localhost:8080/insecure/rs:fill:300:400/plain/https://example.com/image.jpg
```

### 调整大小

```
# 填充到300x400
http://localhost:8080/insecure/rs:fill:300:400/plain/https://example.com/image.jpg

# 适应到300x400
http://localhost:8080/insecure/rs:fit:300:400/plain/https://example.com/image.jpg

# 裁剪到300x400
http://localhost:8080/insecure/rs:fill:300:400/g:nowe/plain/https://example.com/image.jpg
```

### 格式转换

```
# 转换为WebP
http://localhost:8080/insecure/rs:fill:300:400/plain/https://example.com/image.jpg@webp

# 转换为AVIF
http://localhost:8080/insecure/rs:fill:300:400/plain/https://example.com/image.jpg@avif

# 转换为PNG
http://localhost:8080/insecure/rs:fill:300:400/plain/https://example.com/image.jpg@png
```

### 质量控制

```
# 质量为80%
http://localhost:8080/insecure/q:80/plain/https://example.com/image.jpg
```

### 裁剪

```
# 从中心裁剪
http://localhost:8080/insecure/c:100:100:500:500/plain/https://example.com/image.jpg
```

### 水印

```
# 添加水印
http://localhost:8080/insecure/wm:0.5:ce:0:0:0.5/plain/https://example.com/image.jpg
```

### 签名生成

```python
import base64
import hmac
import hashlib

def sign_url(key, salt, path):
    key_bin = bytes.fromhex(key)
    salt_bin = bytes.fromhex(salt)

    msg = salt_bin + path.encode()
    signature = hmac.new(key_bin, msg, hashlib.sha256).digest()

    return base64.urlsafe_b64encode(signature).rstrip(b'=').decode()

# 使用
signature = sign_url(
    "your_secret_key_hex",
    "your_salt_hex",
    "/rs:fill:300:400/plain/https://example.com/image.jpg"
)

url = f"http://localhost:8080/{signature}/rs:fill:300:400/plain/https://example.com/image.jpg"
```

## 健康检查

```bash
curl http://localhost:8080/health
```

## 选项参考

| 选项 | 说明 |
|------|------|
| rs:fit:w:h | 适应大小 |
| rs:fill:w:h | 填充大小 |
| rs:auto:w:h | 自动调整 |
| q:n | 质量 (1-100) |
| g:type | 重力 (ce, no, ea, etc.) |
| @format | 输出格式 |

## 持久化

- imgproxy_images: 源图片目录
