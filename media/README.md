# 媒体服务

照片、视频管理服务。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| photoprism/ | PhotoPrism | 2342 | AI照片管理 |
| immich/ | Immich | 2283 | 照片视频管理 |
| navidrome/ | Navirome | 3443 | 视频监控NVR |

## PhotoPrism

基于AI的照片管理应用。

### 特点

- AI自动分类
- 人脸识别
- 地理定位
- 全文搜索
- 相册管理

### 端口

| 端口 | 说明 |
|------|------|
| 2342 | Web界面 |

### 环境变量

| 变量 | 默认值 |
|------|--------|
| PHOTOPRISM_ADMIN_PASSWORD | admin123456 |
| PHOTOPRISM_SITE_URL | http://localhost:2342 |

### 使用

1. 访问 http://localhost:2342
2. 使用 admin / admin123456 登录
3. 上传照片

### 存储目录

- `./originals/`: 原始照片 (只读挂载)
- `./storage/`: PhotoPrism数据
- `./import/`: 导入目录

## Immich

高性能的自托管照片和视频管理解决方案。

### 组件

| 服务 | 端口 | 说明 |
|------|------|------|
| immich-server | 3001 | API服务 |
| immich-microservices | - | 后台任务 |
| immich-web | 2283 | Web界面 |
| immich-db | 5432 | PostgreSQL |
| immich-redis | 6379 | Redis |
| immich-ml | 3003 | 机器学习 |

### 使用

1. 访问 http://localhost:2283
2. 创建账户
3. 上传照片/视频

### 特点

- 移动端App支持
- AI人脸识别
- 相册共享
- 地图视图
- 硬件加速

### 环境变量

| 变量 | 默认值 |
|------|--------|
| DB_PASSWORD | immich123456 |
| REDIS_HOSTNAME | immich-redis |

## Navirome

视频监控NVR系统。

### 特点

- RTSP支持
- 运动检测
- 录制管理
- Web界面

### 端口

| 端口 | 说明 |
|------|------|
| 3443 | Web界面 |

## 持久化

- photoprism_data: PhotoPrism配置
- immich_db_data: Immich数据库
