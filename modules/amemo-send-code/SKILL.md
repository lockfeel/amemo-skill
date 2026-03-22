---
name: amemo-send-code
description: >
  amemo 发送验证码模块。当用户需要向手机发送验证码时使用，通常在登录前调用。
  触发词：发送验证码、验证码、send code、获取验证码。
---

# amemo-send-code — 发送验证码

## 接口信息

- **路由**: POST http://127.0.0.1:8092/send-code
- **Bean**: LoginBean（自动获取客户端 IP）
- **Content-Type**: application/json

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| phone | str | 是 | 手机号 |
| code | str | 否 | 填 null 或省略 |

## 请求示例

```bash
curl -X POST http://127.0.0.1:8092/send-code \
  -H "Content-Type: application/json" \
  -d '{"phone": "13800138000", "code": null}'
```

## 响应示例

```json
{"code": 200, "desc": "success", "data": "验证码已发送"}
```

## 注意事项

- 此接口无需 userToken，可直接调用
- 调用后提示用户查看手机验证码，再调用 `amemo-login` 完成登录
