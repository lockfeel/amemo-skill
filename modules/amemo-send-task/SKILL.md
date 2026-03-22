---
name: amemo-send-task
description: >
  amemo 发送任务模块。当用户需要将任务推送给相关人员（如通过邮件通知）时使用。
  触发词：发送任务、推送任务、send task、通知任务、分发任务。
---

# amemo-send-task — 发送任务

## 接口信息

- **路由**: POST http://127.0.0.1:8092/send-task
- **Bean**: TaskBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| taskId | str | 否 | 要发送的任务 ID |
| taskTitle | str | 否 | 任务标题 |
| taskExplain | str | 否 | 任务说明 |
| taskTime | str | 否 | 任务时间 |
| taskEmail | list[str] | 否 | 接收通知的邮箱列表 |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/send-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskId": "1", "taskEmail": ["a@example.com", "b@example.com"]}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "..."}
```

## 注意事项

- 此接口用于将任务通知推送给指定邮箱
- taskEmail 为字符串数组，可同时通知多人
- 必须携带有效的 userToken
