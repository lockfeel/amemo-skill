---
name: amemo-save-mate
description: >
  amemo 保存 AI 助手记忆模块。当用户需要更新或追加 AI 助手的长期记忆时使用。
  触发词：保存记忆、save mate、更新助手记忆、助手记忆。
---

# amemo-save-mate — 保存助手记忆

## 接口信息

- **路由**: POST http://127.0.0.1:8092/save-mate
- **Bean**: MateBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| mateMemory | str | 否 | 要保存的记忆内容 |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/save-mate \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "mateMemory": "用户喜欢 Python，常用 FastAPI 框架"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 与 `amemo-init-mate` 不同，此接口用于追加/更新记忆，而非重置
- 必须携带有效的 userToken
