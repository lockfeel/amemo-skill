---
name: amemo-find-data
description: >
  amemo 查询数据模块。当用户需要按类型查询 amemo 中存储的数据时使用。
---

# amemo-find-data — 查询数据

## 接口信息

- **路由**: POST http://127.0.0.1:8092/find-data
- **Bean**: DataBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken` 和 `dataType` 必填且有值，不可传 `null`。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证 |
| dataType | str | **是** | 数据类型（步数/睡眠/血氧/血压/心率/消耗，不能为空） |

## 请求示例

```bash
# 按类型查询
curl -X POST http://127.0.0.1:8092/find-data \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "dataType": "步数"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": [...]}
```
