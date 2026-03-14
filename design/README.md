# 设计工具

在线设计协作工具。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| penpot/ | Penpot | 9001 | 设计协作平台 |
| excalidraw/ | Excalidraw | 8080 | 手绘风格白板 |

## Penpot

开源的设计协作平台，### 端口

| 端口 | 说明 |
|------|------|
| 9001 | Web界面 |

### 组件

- Frontend: Web前端
- Backend: API服务
- PostgreSQL: 数据库
- Redis: 缓存

### 使用

1. 访问 http://localhost:9001
2. 创建账户
3. 开始设计

### 环境变量

| 变量 | 默认值 |
|------|--------|
| PENPOT_PUBLIC_URI | http://localhost:9001 |
| PENPOT_FLAGS | enable-registration |

## Excalidraw

手绘风格的白板工具，### 特点

- 手绘风格
- 实时协作
- 导出PNG/SVG
- 离线支持

### 使用

1. 访问 http://localhost:8080
2. 直接开始绘制

### 无需登录

Excalidraw 无需账户即可使用。

## 持久化

- penpot_data: Penpot数据
- penpot_db_data: 数据库数据
