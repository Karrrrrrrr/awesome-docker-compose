# Meilisearch

Meilisearch 是一个轻量级、高性能的搜索引擎，提供开箱即用的搜索体验，支持拼写容错、同义词、多语言分词等功能。

## 快速启动

```bash
cd meilisearch && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 7700 | HTTP API & Web UI |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| MEILI_PORT | 7700 |
| MEILI_MASTER_KEY | meilisearch123456 |

## 访问界面

启动后访问 http://localhost:7700 查看 Meilisearch Web UI。

## 使用示例

### 创建索引并添加文档

```bash
# 创建索引并添加文档
curl -X POST 'http://localhost:7700/indexes/movies/documents' \
  -H 'Authorization: Bearer meilisearch123456' \
  -H 'Content-Type: application/json' \
  --data-binary '[
    { "id": 1, "title": "Carol", "genres": ["Romance", "Drama"] },
    { "id": 2, "title": "Wonder Woman", "genres": ["Action", "Adventure"] },
    { "id": 3, "title": "Life of Pi", "genres": ["Adventure", "Drama"] }
  ]'
```

### 搜索

```bash
curl -X POST 'http://localhost:7700/indexes/movies/search' \
  -H 'Authorization: Bearer meilisearch123456' \
  -H 'Content-Type: application/json' \
  --data-binary '{ "q": "car" }'
```

### Python

```python
import meilisearch

client = meilisearch.Client('http://localhost:7700', 'meilisearch123456')

# 创建索引
index = client.index('movies')

# 添加文档
index.add_documents([
    {'id': 1, 'title': 'Carol', 'genres': ['Romance', 'Drama']},
    {'id': 2, 'title': 'Wonder Woman', 'genres': ['Action', 'Adventure']}
])

# 搜索
results = index.search('car')
print(results)
```

### JavaScript/Node.js

```javascript
const { MeiliSearch } = require('meilisearch')

const client = new MeiliSearch({
  host: 'http://localhost:7700',
  apiKey: 'meilisearch123456',
})

// 添加文档
const index = client.index('movies')
await index.addDocuments([
  { id: 1, title: 'Carol', genres: ['Romance', 'Drama'] },
  { id: 2, title: 'Wonder Woman', genres: ['Action', 'Adventure'] }
])

// 搜索
const searchResults = await index.search('car')
console.log(searchResults)
```

### Go

```go
import "github.com/meilisearch/meilisearch-go"

client := meilisearch.NewClient(meilisearch.ClientConfig{
    Host:    "http://localhost:7700",
    APIKey:  "meilisearch123456",
})

index := client.Index("movies")

// 添加文档
documents := []map[string]interface{}{
    {"id": 1, "title": "Carol", "genres": []string{"Romance", "Drama"}},
}
index.AddDocuments(documents)

// 搜索
searchRes, _ := index.Search("car", &meilisearch.SearchRequest{})
```

## 功能特点

- **拼写容错**: 自动处理拼写错误
- **同义词支持**: 配置同义词提升搜索体验
- **多语言分词**: 支持中文、日文、韩文等
- **排序和过滤**: 支持多维度排序和过滤
- **分面搜索**: 支持分类导航
- **地理搜索**: 支持地理位置搜索
- **高亮显示**: 搜索结果高亮

## 配置搜索设置

```bash
# 设置可搜索字段
curl -X PATCH 'http://localhost:7700/indexes/movies/settings' \
  -H 'Authorization: Bearer meilisearch123456' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "searchableAttributes": ["title", "genres"],
    "filterableAttributes": ["genres"],
    "sortableAttributes": ["title"]
  }'
```

## 与 Elasticsearch 对比

| 特性 | Meilisearch | Elasticsearch |
|------|-------------|---------------|
| 部署复杂度 | 简单 | 复杂 |
| 资源占用 | 低 | 高 |
| 默认配置 | 开箱即用 | 需配置 |
| 拼写容错 | 默认开启 | 需配置 |
| 中文支持 | 好 | 好 |
| 分布式 | 单节点 | 集群 |
| 适用场景 | 中小型应用 | 大型企业 |

## 健康检查

```bash
curl http://localhost:7700/health
```

## 常用 API

```bash
# 获取索引列表
curl -H 'Authorization: Bearer meilisearch123456' \
  'http://localhost:7700/indexes'

# 获取索引信息
curl -H 'Authorization: Bearer meilisearch123456' \
  'http://localhost:7700/indexes/movies'

# 删除索引
curl -X DELETE -H 'Authorization: Bearer meilisearch123456' \
  'http://localhost:7700/indexes/movies'

# 获取任务状态
curl -H 'Authorization: Bearer meilisearch123456' \
  'http://localhost:7700/tasks'
```

## 相关链接

- 官网: https://www.meilisearch.com/
- GitHub: https://github.com/meilisearch/meilisearch
- 文档: https://www.meilisearch.com/docs
