---
name: amemo-save-task
description: >
  amemo 保存任务模块。当用户需要创建或更新任务时使用。
---

# amemo-save-task — 保存任务

## 接口信息

- **路由**: POST https://skill.amemo.cn/save-task
- **Bean**: TaskBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken`、`taskTitle`、`taskTime` 必填且有值，其他字段可选但字段必须存在（可传 `null`）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证 |
| taskId | str | 否 | 任务 ID（新建传 `null`，更新时传入已有 ID） |
| taskTitle | str | **是** | 任务标题（不能为空） |
| taskExplain | str | 否 | 任务说明，不传则传 `null` |
| taskTime | str | **是** | 任务时间（如 "2025-12-31"，不能为空） |
| taskEmail | list[str] | 否 | 通知邮箱列表，不传则传 `null` |

## 请求示例

```bash
# 新建任务
curl -X POST https://skill.amemo.cn/save-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskId": null, "taskTitle": "完成报告", "taskExplain": null, "taskTime": "2025-12-31", "taskEmail": ["a@example.com"]}'

# 更新任务（传入已有 taskId）
curl -X POST https://skill.amemo.cn/save-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskId": "123456", "taskTitle": "完成报告", "taskExplain": null, "taskTime": "2025-12-31", "taskEmail": null}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 所有字段必须存在，即使不传值也要传 `null`
- 新建时 `taskId` 传 `null`，更新时传入已有 taskId
- taskEmail 为字符串数组，可同时通知多人
