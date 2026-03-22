---
name: amemo-save-memo
description: >
  amemo 保存备忘录模块。当用户需要创建或更新备忘录时使用。
  触发词：保存备忘、写备忘、save memo、创建备忘录、记下来、记录。
---

# amemo-save-memo — 保存备忘录

## 接口信息

- **路由**: POST http://127.0.0.1:8092/save-memo
- **Bean**: MemoBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| memoId | str | 否 | 备忘录 ID（更新时传入） |
| memoTitle | str | 否 | 备忘录标题 |
| memoContent | str | 否 | 备忘录内容 |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/save-memo \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "memoTitle": "开会记录", "memoContent": "讨论了Q2计划"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 新建时可不传 memoId，更新时需传入已有 memoId
- 必须携带有效的 userToken
