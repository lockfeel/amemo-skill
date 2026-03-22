---
name: amemo-save-mate
description: >
  amemo 保存 AI 助手记忆模块。当用户需要更新或追加 AI 助手的长期记忆时使用。
---

# amemo-save-mate — 保存助手记忆

## 接口信息

- **路由**: POST https://skill.amemo.cn/save-mate
- **Bean**: MateBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken` 和 `mateMemory` 必填且有值。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证 |
| mateMemory | str | **是** | 要保存的记忆内容（不能为空） |

## 请求示例

```bash
# 保存记忆
curl -X POST https://skill.amemo.cn/save-mate \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "mateMemory": "用户喜欢 Python，常用 FastAPI 框架"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- `userToken` 和 `mateMemory` 都必须有值，不能为空
- 与 `amemo-init-mate` 不同，此接口用于追加/更新记忆，而非重置
- 必须携带有效的 userToken
