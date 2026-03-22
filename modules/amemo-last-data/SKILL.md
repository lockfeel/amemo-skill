---
name: amemo-last-data
description: >
  amemo 查询最新数据模块。当用户需要获取 amemo 中最新的数据记录时使用。
---

# amemo-last-data — 查询最新数据

## 接口信息

- **路由**: POST http://127.0.0.1:8092/last-data
- **Bean**: DataBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| dataType | str | 否 | 数据类型（用于筛选最新记录） |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/last-data \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>"}'

# 按类型获取最新
curl -X POST http://127.0.0.1:8092/last-data \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "dataType": "report"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": {...}}
```

## 注意事项

- 与 `amemo-find-data` 不同，此接口只返回最新的记录
- 不传 dataType 则返回所有类型中最新的数据
