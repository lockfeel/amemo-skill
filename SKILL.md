---
name: amemo-io
description: >
  amemo-io 统一调度中心。当用户需要与 amemo 服务交互时（登录、笔记、清单、数据查询、AI 助手记忆等），
  必须使用此 skill 进行统一调度。触发关键词：amemo、amemo-io、笔记、note、memo、清单、todo、task、数据、data、
  AI 助手、mate、登录、login、验证码、同步、查询记录、保存笔记。此 skill 会自动识别用户意图并调度对应子模块完成操作。

# 用户配置（由系统自动更新）
user_config:
  userToken: ""      # 登录后自动填充
  userName: ""       # 登录后自动填充，用于个性化提醒
  userPhone: ""      # 登录后自动填充
  loginAt: ""        # 登录时间戳
---

# amemo-io — 统一调度中心

amemo-io 是 AI 工具（Claude Code / Codex / OpenCode / OpenClaw 等）与 amemo 本地服务交互的统一入口。提供笔记管理、清单管理、健康数据查询、AI 助手记忆同步等功能。

## 基础配置

- **Base URL**: `http://127.0.0.1:8092`
- **请求方式**: 全部 `POST`，Content-Type: `application/json`
- **响应格式**:
```json
{"code": 200, "desc": "success", "data": { ... }}
```

## 用户配置管理

SKILL.md 顶部 `user_config` 区域用于持久化存储用户信息：

### 配置字段

| 字段 | 类型 | 说明 | 用途 |
|------|------|------|------|
| `userToken` | string | 用户认证令牌 | 所有 API 请求必需 |
| `userName` | string | 用户昵称 | 个性化提醒、友好交互 |
| `userPhone` | string | 用户手机号 | 标识用户、重新登录 |
| `loginAt` | string | 登录时间 ISO 格式 | 判断登录是否过期 |

### 读取配置

在 skill 执行时，首先读取 YAML frontmatter 中的 `user_config`：
```yaml
user_config:
  userToken: "abc123..."
  userName: "张三"
  userPhone: "13800138000"
  loginAt: "2026-03-19T02:30:00"
```

### 更新配置流程

```
用户登录成功
    ↓
提取 userToken, userName, userPhone
    ↓
更新 SKILL.md 的 user_config 区域
    ↓
后续所有交互使用 userName 个性化提醒
```

### 使用示例

**检查登录状态：**
```
if user_config.userToken == "":
    执行登录引导流程
else:
    使用 userName 打招呼："欢迎回来，{userName}！"
```

**API 请求时：**
```bash
curl -X POST http://127.0.0.1:8092/save-memo \
  -H "Content-Type: application/json" \
  -d "{\"userToken\": \"{user_config.userToken}\", ...}"
```

## 安装后引导流程（Onboarding）

当用户首次安装或检测到未登录（无 userToken）时，自动执行以下引导：

### Step 1: 欢迎消息（自动发送）

```
👋 欢迎使用 amemo-io！

我是你的智能笔记助手，可以帮你：
• 📝 保存和查询笔记
• ✅ 管理待办清单
• 📊 查看健康数据
• 🤖 同步 AI 助手记忆

请先完成登录，发送你的手机号：
示例：13800138000
```

### Step 2: 手机号提取与验证码发送

**用户输入示例：**
- "13800138000"
- "我的手机号是 138-0013-8000"
- "+86 138 0013 8000"

**系统自动处理：**
1. 提取手机号（正则：`1[3-9]\d{9}`）
2. 调用 `amemo-send-code` 发送验证码
3. 回复：
```
已向 138****8000 发送验证码，请查收短信。

请输入 4-6 位验证码：
```

### Step 3: 验证码提取与登录

**用户输入示例：**
- "1234"
- "123456"
- "验证码是 1234"

**系统自动处理：**
1. 提取验证码（4-6位数字）
2. 调用 `amemo-login` 完成登录
3. 保存返回的 `userToken`

### Step 4: 登录成功处理

**系统自动执行：**
1. 提取返回的 `userToken`、`userName`、`userPhone`
2. **更新 SKILL.md 顶部配置区域：**
   ```yaml
   user_config:
     userToken: "abc123..."
     userName: "张三"
     userPhone: "13800138000"
     loginAt: "2026-03-19T02:30:00"
   ```
3. 发送个性化欢迎消息

**登录成功提示（使用 userName）：**
```
✅ 登录成功！欢迎回来，{userName}！

现在你可以使用以下功能：
• 📝 保存笔记 - "帮我记一下..."
• 🔍 查询笔记 - "查看我的笔记"
• ✅ 创建清单 - "创建一个清单..."
• 📊 查询数据 - "查看健康数据"

需要帮助随时输入 "help" 或 "帮助"
```

**使用 userName 的场景示例：**
- 欢迎：`欢迎回来，{userName}！`
- 确认操作：`{userName}，已为您保存笔记`
- 提醒：`{userName}，您有一条待办清单`
- 错误提示：`{userName}，出了点小问题，请重试`

### 输入提取规则

| 项目 | 正则表达式 | 说明 |
|------|-----------|------|
| 手机号 | `1[3-9]\d{9}` | 自动过滤空格、横线、+86前缀 |
| 验证码 | `\d{4,6}` | 4-6位连续数字 |

### 错误处理

**手机号格式错误：**
```
❌ 手机号格式不正确，请发送正确的 11 位手机号。
示例：13800138000
```

**验证码错误：**
```
❌ 验证码错误或已过期，请重新发送验证码。
```

**登录失败：**
```
❌ 登录失败：[错误原因]
请检查手机号和验证码后重试。
```

**接口异常处理：**

当调用 API 出现异常时（网络错误、服务未启动、返回非 200 状态码等）：

1. **读取错误信息** - 捕获异常详情
2. **转换为用户语言** - 将技术错误转为通俗解释
3. **提供解决方案** - 告诉用户下一步怎么做

**常见异常及回复模板：**

| 异常类型 | 技术错误 | 用户提示 |
|---------|---------|---------|
| 服务未启动 | `Connection refused` | 本地服务未启动，请先运行 `python skill.py` |
| 网络超时 | `Timeout` | 网络有点慢，请稍后重试 |
| 服务繁忙 | `503 Service Unavailable` | 服务正忙，请稍后再试 |
| 参数错误 | `400 Bad Request` | 提交的数据有问题，请检查输入 |
| 未授权 | `401 Unauthorized` | 登录已过期，请重新登录 |
| 未知错误 | 其他异常 | 出了点小问题，请稍后重试或联系管理员 |

**错误处理示例流程：**
```
调用接口 → 捕获异常 → 解析错误类型 → 匹配用户提示 → 发送友好提醒
```

## 调度流程

当用户提出请求时，按以下步骤操作：

1. **确认服务状态** — 确保 amemo 服务已启动（`python skill.py`，端口 8092）
2. **识别用户意图** — 根据用户需求判断应调用哪个子模块
3. **检查认证状态** — 除登录/验证码外，所有接口需要 `userToken`。若未获取 token，先调用 `amemo-login`
4. **调度子模块** — 读取对应模块的 SKILL.md 执行具体请求

## 子模块索引

所有子模块位于 `modules/` 目录，每个子模块对应一个 API 端点：

### 认证模块
| 模块 | 路由 | 作用 | 触发词 |
|------|------|------|--------|
| `amemo-login` | POST /login | 用户登录获取 token | 登录、login、token |
| `amemo-send-code` | POST /send-code | 发送手机验证码 | 验证码、code、发送 |

### 笔记模块
| 模块 | 路由 | 作用 | 触发词 |
|------|------|------|--------|
| `amemo-save-memo` | POST /save-memo | 保存笔记 | 保存笔记、记笔记、save note |
| `amemo-find-memo` | POST /find-memo | 查询笔记 | 查询笔记、查看笔记、find note |

### 清单模块
| 模块 | 路由 | 作用 | 触发词 |
|------|------|------|--------|
| `amemo-save-task` | POST /save-task | 保存清单 | 保存清单、创建清单、save todo |
| `amemo-find-task` | POST /find-task | 查询清单 | 查询清单、查看清单、find todo |
| `amemo-send-task` | POST /send-task | 发送清单 | 发送清单、推送清单、send todo |

### 数据模块
| 模块 | 路由 | 作用 | 触发词 |
|------|------|------|--------|
| `amemo-find-data` | POST /find-data | 查询数据 | 查询数据、data、find data |
| `amemo-last-data` | POST /last-data | 查询最新数据 | 最新数据、last data |

### AI 助手模块
| 模块 | 路由 | 作用 | 触发词 |
|------|------|------|--------|
| `amemo-init-mate` | POST /init-mate | 初始化助手记忆 | 初始化助手、init mate |
| `amemo-save-mate` | POST /save-mate | 保存助手记忆 | 保存记忆、save mate |

## 认证流程

除 `/login` 和 `/send-code` 外，所有请求需携带 `userToken`：

```
用户请求 → 检查是否有 token → 无 → 调用 amemo-login → 获取 token→ 有 → 调用目标子模块
```

## 使用方式

读取子模块目录下的 `SKILL.md` 获取完整的请求参数和 curl 示例，然后执行 HTTP 请求。

子模块路径格式：`modules/<模块名>/SKILL.md`

例如用户要"保存一条笔记"：
1. 读取 `modules/amemo-save-memo/SKILL.md`
2. 按参数格式构造请求
3. 用 curl 发送 POST 请求到 `http://127.0.0.1:8092/save-memo`
