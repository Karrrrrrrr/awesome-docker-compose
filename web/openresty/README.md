# OpenResty

OpenResty 是一个基于 Nginx 的 Web 平台，集成了 LuaJIT。

## 快速启动

```bash
cd openresty && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 80 | HTTP |
| 443 | HTTPS |

## 配置文件

| 文件 | 说明 |
|------|------|
| nginx.conf | 主配置 |
| conf.d/*.conf | 站点配置 |
| lua/*.lua | Lua脚本 |

## Lua示例

### 基本响应

```lua
-- conf.d/lua.conf
location /lua {
    default_type 'text/plain';
    content_by_lua_block {
        ngx.say('Hello, OpenResty!')
    }
}
```

### 访问Redis

```lua
location /redis {
    default_type 'text/plain';
    content_by_lua_block {
        local redis = require "resty.redis"
        local red = redis:new()

        red:connect("redis", 6379)

        local res, err = red:get("mykey")
        if res == ngx.null then
            ngx.say("key not found")
        else
            ngx.say("value: ", res)
        end
    }
}
```

### 访问MySQL

```lua
location /mysql {
    default_type 'text/plain';
    content_by_lua_block {
        local mysql = require "resty.mysql"
        local db, err = mysql:new()

        db:connect({
            host = "mysql",
            port = 3306,
            database = "app",
            user = "root",
            password = "root123456"
        })

        local res, err = db:query("SELECT * FROM users LIMIT 10")
        ngx.say(require "cjson".encode(res))
    }
}
```

### JSON响应

```lua
location /api {
    default_type 'application/json';
    content_by_lua_block {
        local cjson = require "cjson"
        local data = {
            status = "ok",
            message = "Hello API"
        }
        ngx.say(cjson.encode(data))
    }
}
```

### 请求限制

```nginx
# nginx.conf
lua_shared_dict limit_req_store 100m;

# conf.d/limit.conf
location /api {
    access_by_lua_block {
        local limit_req = require "resty.limit.req"
        local lim, err = limit_req.new("limit_req_store", 100, 200)

        local delay, err = lim:incoming(ngx.var.binary_remote_addr, true)
        if not delay then
            if err == "rejected" then
                ngx.exit(429)
            end
        end
    }
}
```

### 缓存

```lua
lua_shared_dict my_cache 128m;

location /cached {
    content_by_lua_block {
        local cache = ngx.shared.my_cache
        local key = ngx.var.uri

        local value = cache:get(key)
        if not value then
            value = "expensive_result"
            cache:set(key, value, 60)
        end

        ngx.say(value)
    end
```

## 常用命令

```bash
# 测试配置
docker exec openresty nginx -t

# 重载配置
docker exec openresty nginx -s reload

# 查看日志
docker logs -f openresty

# 进入容器
docker exec -it openresty bash
```

## 持久化

- openresty_logs: 日志
