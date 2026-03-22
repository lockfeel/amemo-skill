---
name: amemo-find-data
description: >
  amemo 查询数据模块。当用户需要按类型查询 amemo 中存储的数据时使用。
  触发词：查询数据、find data、查看数据、data、数据查询。
---

# amemo-find-data — 查询数据

## 接口信息

- **路由**: POST http://127.0.0.1:8092/find-data
- **Bean**: DataBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| dataType | str | 否 | 数据类型（用于筛选） |

## 请求示例

```bash
# 查询所有数据
curl -X POST http://127.0.0.1:8092/find-data \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>"}'

# 按类型查询
curl -X POST http://127.0.0.1:8092/find-data \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "dataType": "some_type"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": [...]}
```
