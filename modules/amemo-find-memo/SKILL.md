---
name: amemo-find-memo
description: >
  amemo 查询备忘录模块。当用户需要查看、搜索已有备忘录时使用。
---

# amemo-find-memo — 查询备忘录

## 接口信息

- **路由**: POST http://127.0.0.1:8092/find-memo
- **Bean**: MemoBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| memoId | str | 否 | 按 ID 精确查询 |
| memoTitle | str | 否 | 按标题模糊查询 |
| memoContent | str | 否 | 按内容模糊查询 |

## 请求示例

```bash
# 查询所有备忘录
curl -X POST http://127.0.0.1:8092/find-memo \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>"}'

# 按标题查询
curl -X POST http://127.0.0.1:8092/find-memo \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "memoTitle": "开会"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": {}}
```
