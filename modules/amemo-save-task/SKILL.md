---
name: amemo-save-task
description: >
  amemo 保存任务模块。当用户需要创建或更新任务时使用。
  触发词：保存任务、创建任务、save task、新建任务、添加任务。
---

# amemo-save-task — 保存任务

## 接口信息

- **路由**: POST http://127.0.0.1:8092/save-task
- **Bean**: TaskBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| taskId | str | 否 | 任务 ID（更新时传入） |
| taskTitle | str | 否 | 任务标题 |
| taskExplain | str | 否 | 任务说明 |
| taskTime | str | 否 | 任务时间（如 "2025-12-31"） |
| taskEmail | list[str] | 否 | 通知邮箱列表 |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/save-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskTitle": "完成报告", "taskTime": "2025-12-31", "taskEmail": ["a@example.com"]}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 新建时可不传 taskId，更新时需传入已有 taskId
- taskEmail 为字符串数组，可同时通知多人
