---
name: amemo-save-memo
description: >
  amemo 保存备忘录模块。当用户需要创建或更新备忘录时使用。
---

# amemo-save-memo — 保存备忘录

## 接口信息

- **路由**: POST http://127.0.0.1:8092/save-memo
- **Bean**: MemoBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken`、`memoTitle`、`memoContent` 必填且有值，`memoId` 可选但字段必须存在（新建传 `null`）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证 |
| memoId | str | 否 | 备忘录 ID（新建传 `null`，更新时传入已有 ID） |
| memoTitle | str | **是** | 备忘录标题（不能为空） |
| memoContent | str | **是** | 备忘录内容（不能为空） |

## 请求示例

```bash
# 新建备忘录
curl -X POST http://127.0.0.1:8092/save-memo \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "memoId": null, "memoTitle": "开会记录", "memoContent": "讨论了Q2计划"}'

# 更新备忘录（传入已有 memoId）
curl -X POST http://127.0.0.1:8092/save-memo \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "memoId": "123456", "memoTitle": "开会记录", "memoContent": "更新了内容"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 所有字段必须存在，即使不传值也要传 `null`
- 新建时 `memoId` 传 `null`，更新时传入已有 memoId
- 必须携带有效的 userToken
