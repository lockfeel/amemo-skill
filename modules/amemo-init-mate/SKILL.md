---
name: amemo-init-mate
description: >
  amemo 初始化 AI 助手模块。当用户首次使用或需要重置 AI 助手记忆时使用。
---

# amemo-init-mate — 初始化 AI 助手

## 接口信息

- **路由**: POST http://127.0.0.1:8092/init-mate
- **Bean**: MateBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken` 必填，`mateMemory` 可选但字段必须存在（可传 `null`）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证（通过 amemo-login 获取） |
| mateMemory | str | 否 | 初始记忆内容，不传则传 `null` |

## 请求示例

```bash
# 初始化（不传记忆内容）
curl -X POST http://127.0.0.1:8092/init-mate \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "mateMemory": null}'

# 初始化并设置记忆
curl -X POST http://127.0.0.1:8092/init-mate \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "mateMemory": "用户偏好：喜欢简洁风格"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 所有字段必须存在，即使不传值也要传 `null`
- 必须先通过 `amemo-login` 获取 userToken
- `mateMemory` 为可选，用于设定助手初始记忆
