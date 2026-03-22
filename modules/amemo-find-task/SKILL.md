---
name: amemo-find-task
description: >
  amemo 查询任务模块。当用户需要查看、搜索已有任务时使用。
  触发词：查询任务、查看任务、find task、列出任务、搜索任务。
---

# amemo-find-task — 查询任务

## 接口信息

- **路由**: POST http://127.0.0.1:8092/find-task
- **Bean**: TaskBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | 是 | 用户登录凭证 |
| taskId | str | 否 | 按 ID 精确查询 |
| taskTitle | str | 否 | 按标题模糊查询 |
| taskExplain | str | 否 | 按说明模糊查询 |
| taskTime | str | 否 | 按时间筛选 |
| taskEmail | list[str] | 否 | 按邮箱筛选 |

## 请求示例

```bash
# 查询所有任务
curl -X POST http://127.0.0.1:8092/find-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>"}'

# 按标题查询
curl -X POST http://127.0.0.1:8092/find-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskTitle": "报告"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": [{"taskId": "1", "taskTitle": "...", "taskTime": "..."}]}
```
