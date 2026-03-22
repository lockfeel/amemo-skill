---
name: amemo-login
description: >
  amemo 用户登录模块。当用户需要登录 amemo 获取 userToken 时使用。
  触发词：登录、login、获取token、userToken。
---

# amemo-login — 用户登录

## 接口信息

- **路由**: POST http://127.0.0.1:8092/login
- **Bean**: LoginBean
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| phone | str | 是 | 手机号 |
| code | str | 是 | 验证码（先通过 amemo-send-code 获取） |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/login \
  -H "Content-Type: application/json" \
  -d '{"phone": "13800138000", "code": "123456"}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": {"userToken": "xxx..."}}
```

## 注意事项

- 调用前需先通过 `amemo-send-code` 获取验证码
- 返回的 `userToken` 需保存，后续所有接口调用均需携带此 token
