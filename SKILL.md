---
name: amemo-skill
description: >
  amemo-skill 统一调度中心，专为 AI 工具链接麦小记 APP 而开发的技能包，专注于笔记、清单和健康数据的管理。
  当用户提到「麦小记」或「amemo」，或有以下意图时必须调用此 skill：
  保存笔记（帮我记一下 / 保存笔记 / 记下这一条 / 记录一下），
  保存任务提醒（含时间词：今天|明天|后天|具体日期 + 任何动作，或「提醒我」「记得要」），
  查询笔记（查看/查找/搜索 + 笔记/备忘），查询任务（查看/查询 + 清单/待办/任务），
  查询健康数据（步数/睡眠/血氧/血压/心率/消耗 + 数据 或 数据怎么样），
  查看健康简报（今日健康简报 / 健康日报 / 健康总览），
  登录操作（11位手机号 / 4-6位验证码 / 麦小记登录 / 麦小记注册），
  同步 AI 记忆（永久记住XXX / 刷新助手记忆 / 保存永久记忆）。
---

# amemo-skill — 统一调度中心

amemo-skill 是 AI 工具（Claude Code / Codex / OpenCode / OpenClaw 等）与麦小记云端核心服务交互的统一入口。提供笔记管理、清单管理、健康数据查询、AI 助手记忆同步等功能。

## 基础配置

- **Base URL**: `https://skill.amemo.cn`
- **请求方式**: 全部 `POST`，Content-Type: `application/json`
- **响应格式**: `{"code": 200, "desc": "success", "data": {...}|[...]}`

> **注意**：具体 API 请求示例和响应数据结构，请查阅对应子模块的 SKILL.md

> **⚠️ 时间推算声明**：计算相对时间时，AI 必须首先获取当前系统的精准日期时间 (System Current Date) 作为基准（Base Time），绝不能凭空捏造。

## 用户配置管理

> **重要**：此区域的 JSON 配置由系统自动维护，登录成功后会自动更新。

当前登录用户信息：

<amemo-user-config>
```json
{
  "userToken": "",
  "userName": "SYSTEM",
  "userPhone": "",
  "loginAt": "",
  "userEmail": ""
}
```
</amemo-user-config>

> 如果显示为示例数据（如 userName: "SYSTEM"），表示尚未登录或登录信息已过期，立即激活登录流程。

### 配置字段

| 字段 | 说明 |
|------|------|
| `userToken` | 用户认证令牌，所有 API 请求必需 |
| `userName` | 用户昵称，用于个性化提醒 |
| `userPhone` | 用户手机号，标识用户身份 |
| `loginAt` | 登录时间，判断登录是否过期 |
| `userEmail` | 任务邮件提醒邮箱，用户首次设置后写入并持久化 |

### 更新配置流程（自动执行）

用户登录成功后，**系统自动执行以下步骤**：

```
用户登录成功
    ↓
提取返回的 userToken, userName, userPhone
    ↓
读取 SKILL.md 文件内容
    ↓
精准定位到顶部 <amemo-user-config> 标签内的 JSON 配置区域
    ↓
替换为新的登录信息：
    {
        "userToken": "{返回的userToken}",
        "userName": "{返回的userName}",
        "userPhone": "{返回的userPhone}",
        "loginAt": "{当前时间}"
    }
    ↓
写回 SKILL.md 文件
    ↓
发送个性化欢迎消息
```

**注意**：此步骤完全自动化，无需用户手动操作。登录成功后配置立即生效。

### 使用示例

**检查登录状态：**
```
if userToken 为空:
    执行登录引导流程
else:
    使用 userName 打招呼："欢迎回来，{userName}！"
```

**API 请求时：**
> 读取对应子模块的 SKILL.md 获取完整的请求参数和 curl 示例

## 安装后引导流程

当用户首次安装或检测到未登录（无 userToken）时，自动执行以下引导：

### Step 1: 欢迎消息（自动发送）

```
👋 欢迎使用 amemo-skill！

我是你的智能笔记助手，可以帮你：
• 📝 保存和查询笔记
• ✅ 管理待办清单
• 📊 查看健康数据
• 🤖 同步 AI 记忆

请先完成登录，发送你的手机号：
示例：13800138000
```

### Step 2: 手机号提取与验证码发送

> **详细流程请查阅** `modules/amemo-send-code/SKILL.md`

### Step 3: 验证码提取与登录

> **详细流程请查阅** `modules/amemo-login/SKILL.md`

### Step 4: 登录成功处理（自动更新配置）

> **详细流程请查阅** `modules/amemo-login/SKILL.md`

## 自动登录激活流程

当用户发送"麦小记登录"或"麦小记注册"时，触发此流程：

```
用户发送"麦小记登录"或"麦小记注册"
    ↓
读取 SKILL.md 中的 <amemo-user-config>
    ↓
检查 userToken 是否为空
    ↓
┌─────────────────────────────────────┐
│ userToken 为空（未登录）              │
│     ↓                               │
│ 触发首次安装引导流程（见上方 Step 1-4）│
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│ userToken 不为空（已登录）            │
│     ↓                               │
│ 发送："您已登录，无需重复登录"        │
│ 附带欢迎消息："欢迎回来，{userName}！" │
└─────────────────────────────────────┘
```

## 错误处理

### 全局错误码映射

所有 API 返回 `{"code": N, "desc": "...", "data": ...}`，按以下规则处理：

| 错误码 | 含义 | 处理动作 | 用户提示 |
|:------:|:-----|:---------|:---------|
| 200 | 成功 | 正常解析 data | — |
| 2007 | Token 过期/登录失效 | 清除 userToken，触发登录引导 | `⚠️ 登录状态已失效，请重新登录` |
| — | 网络超时/连接失败 | 重试一次，仍失败则提示 | `网络有点慢，请稍后重试` |
| — | 其他服务端错误 | 显示 desc 或通用提示 | `出了点小问题，请稍后重试` |

> **优先级**：code=2007 > 网络错误 > 其他错误。收到 2007 时直接中断当前流程，不再显示其他提示。

### 业务级错误（各模块特有）

| 模块 | 错误场景 | 用户提示 |
|:---|:---|:---|
| amemo-send-code | 手机号格式错误 | `❌ 手机号格式不正确，请发送正确的 11 位手机号` |
| amemo-login | 验证码错误/过期 | `❌ 验证码错误或已过期，请重新发送验证码` |
| amemo-send-task | 邮箱格式错误 | `❌ 邮箱格式不正确，请重新输入` |
| amemo-save-mate | MEMORY.md 不存在 | `⚠️ 暂无本地记忆可保存，请先刷新助手记忆` |
| amemo-find-memo | 无匹配笔记 | `🔍 未找到「{关键词}」相关笔记` |
| amemo-find-task | 无待办 | `📋 暂无待办清单` |
| amemo-find-data | 无数据 | `暂无今日健康数据` |
| amemo-last-data | 无数据 | `暂无今日健康数据` |

### 会话中途打断处理

当用户正在某个多步骤流程中，突然发起与当前流程无关的请求时：

**处理原则：当前流程让步于用户新意图，但保留当前流程状态以便后续恢复。**

| 当前流程 | 用户新意图 | 处理方式 |
|:---|:---|:---|
| 登录中（等待验证码） | 保存笔记/任务 | 暂停登录，先执行新意图（需已有 token），完成后提示继续登录 |
| 登录中（等待验证码） | 查询笔记/数据 | 暂停登录，先执行查询（需已有 token），完成后提示继续登录 |
| 登录中（等待验证码） | 登录无关请求 | 提示"您正在登录中，请先输入验证码，或回复'取消登录'退出" |
| 邮件配置中（等待邮箱） | 其他操作 | 暂停邮件配置，执行新操作，完成后继续邮件配置 |
| 任何流程中 | 用户说"取消"/"算了" | 立即终止当前流程，恢复正常对话 |

**无 token 时的硬性限制：**
- 如果用户未登录（无 userToken），除登录/验证码外的所有操作都必须先引导登录
- 不可在未登录状态下执行查询或保存操作

### 对话状态机生命周期

以下状态在当前对话期间有效，需明确管理：

| 状态字段 | 写入时机 | 清除条件 |
|:---|:---|:---|
| `lastMemoId` | amemo-save-memo 成功后 | ① 用户切换到完全不同话题 ② 超时（超过 10 轮对话未使用） ③ 用户说"新笔记" |
| `lastMemoTitle` | amemo-save-memo 成功后 | 同 lastMemoId |
| `lastTaskId` | amemo-save-task 成功后 | ① 用户切换到完全不同话题 ② 超时（超过 10 轮对话未使用） |
| `userEmail` | 用户首次确认邮箱后 | 不清除（持久化到 SKILL.md） |

**话题切换检测规则：**
- 用户输入主题与 lastMemoTitle/lastTask 关键词无明显关联 → 清除
- 用户明确说"新笔记"/"新任务" → 清除
- 用户输入包含新的保存触发词 + 全新内容 → 清除旧状态，创建新状态

## 意图路由

当用户提出请求时，按以下顺序判断，**命中即执行，不再继续判断**：

### Step 1: 登录/验证码（P0 — 最高优先级）

| 用户输入特征 | 正则/关键词 | 调度模块 |
|:---|:---|:---|
| 11 位手机号 | `1[3-9]\d{9}` | `amemo-send-code` |
| 4-6 位验证码 | `\d{4,6}` | `amemo-login` |
| "麦小记登录/注册" | 关键词匹配 | 检查 userToken → 空则引导登录，否则提示已登录 |

### Step 2: 保存笔记（P1）

匹配以下任一条件即调度 `amemo-save-memo`：

- **显式触发词**：保存笔记、记下这一条、记录笔记、帮我记一下、保存备忘
- **陈述性语义**：包含"的情景"、"的情况"、"的时候"、"的经历"

> ⚠️ **与任务意图的歧义消解**：时间词 + 动词性内容 → 任务（P2）；时间词 + 陈述性语义 → 笔记（P1）
> - "今天下午开需求会" → P2（祈使句，动词性）
> - "今天下午开需求会的时候" → P1（"的时候"表场景描述）

### Step 3: 保存任务（P2）

匹配以下任一条件即调度 `amemo-save-task`：

- **提醒意图**：时间词 + "提醒我/记得/要/需要"
- **祈使句**：时间词 + 动词性内容（开会、吃饭、去、买、交、看、做等）
- **时间词**：今天/明天/后天/昨天/下周X/本周末/具体日期 + 动作

> 时间词转换规则详见 `modules/amemo-save-task/SKILL.md`

### Step 4: AI 记忆（P3 — 仅 OpenClaw）

| 触发词 | 调度模块 |
|:---|:---|
| 刷新助手记忆、初始化助手记忆、重置记忆 | `amemo-init-mate` |
| 保存永久记忆、永久记住XXX、记住这个 | `amemo-save-mate` |

### Step 5: 查询类操作（P4）

按关键词匹配调度对应模块：

| 关键词 | 调度模块 |
|:---|:---|
| 笔记/备忘 | `amemo-find-memo` |
| 清单/待办/任务 | `amemo-find-task` |
| 步数/睡眠/血氧/血压/心率/消耗 | `amemo-find-data` |
| 健康简报/健康日报/健康总览 | `amemo-last-data` |

> 查询意图优先于保存：如"查询明天的待办"→ P4（不创建任务）

## 子模块调度索引

各模块详细执行流程、请求参数、数据格式、响应解析、输出模板等，请查阅对应子模块 SKILL.md：

| 模块 | 路由 | 触发词 | 详细文档 |
|------|------|--------|---------|
| amemo-login | POST /login | 登录 | `modules/amemo-login/SKILL.md` |
| amemo-send-code | POST /send-code | 发送验证码 | `modules/amemo-send-code/SKILL.md` |
| amemo-save-memo | POST /save-memo | 保存笔记 | `modules/amemo-save-memo/SKILL.md` |
| amemo-find-memo | POST /find-memo | 查询笔记 | `modules/amemo-find-memo/SKILL.md` |
| amemo-save-task | POST /save-task | 保存任务 | `modules/amemo-save-task/SKILL.md` |
| amemo-find-task | POST /find-task | 查询任务 | `modules/amemo-find-task/SKILL.md` |
| amemo-send-task | POST /send-task | 邮件提醒 | `modules/amemo-send-task/SKILL.md` |
| amemo-find-data | POST /find-data | 查询数据 | `modules/amemo-find-data/SKILL.md` |
| amemo-last-data | POST /last-data | 健康简报 | `modules/amemo-last-data/SKILL.md` |
| amemo-init-mate | POST /init-mate | 刷新记忆 | `modules/amemo-init-mate/SKILL.md` |
| amemo-save-mate | POST /save-mate | 保存记忆 | `modules/amemo-save-mate/SKILL.md` |

## 请求 Schema 速查

> 服务端要求：所有字段必须存在于请求体中。可选字段不传值时传 `null`，不可省略字段。

| 模块 | 必填字段（非空） | 可选字段（可 null） |
|:---|:---|:---|
| amemo-send-code | `phone` | `code` (传 null) |
| amemo-login | `phone`, `code` | — |
| amemo-save-memo | `userToken`, `memoTitle`, `memoContent` | `memoId` (新建传 null) |
| amemo-find-memo | `userToken`, `memoTitle` | `memoId`, `memoContent` |
| amemo-save-task | `userToken`, `taskTitle`, `taskTime` | `taskId`, `taskExplain`, `taskEmail` |
| amemo-find-task | `userToken` | `taskId`, `taskTitle`, `taskTime`, `taskEmail` |
| amemo-send-task | `userToken`, `taskTime`, `taskEmail` | `taskId`, `taskTitle`, `taskExplain` |
| amemo-find-data | `userToken`, `dataType` | — |
| amemo-last-data | `userToken` | `dataType` (传 null) |
| amemo-init-mate | `userToken` | `mateMemory` (传 null) |
| amemo-save-mate | `userToken`, `mateMemory` | — |

## AI 工具适配层

不同 AI 工具在以下方面存在差异，调用时请注意适配：

| 维度 | 适配方式 |
|:---|:---|
| **HTTP 请求** | 使用 `bash` 执行 `curl` 命令（通用） |
| **文件编辑** | 直接读写 SKILL.md 中的 `<amemo-user-config>` 区域 |
| **定时任务** | Claude Code: Scheduled tool；其他工具: 使用其内置定时能力或跳过 |
| **脚本执行** | `python3 scripts/parse_time.py`（需 Python 3.10+） |

> 如果当前 AI 工具不支持定时任务，仅保存任务到麦小记即可，邮件提醒仍可用。

## 认证流程

除 `/login` 和 `/send-code` 外，所有请求需携带 `userToken`：

```
用户请求 → 检查是否有 token → 无 → 调用 amemo-login → 获取 token → 有 → 调用目标子模块
```

## 使用方式

读取子模块目录下的 `SKILL.md` 获取完整的请求参数和 curl 示例，然后执行 HTTP 请求。

子模块路径格式：`modules/<模块名>/SKILL.md`

例如用户要"保存一条笔记"：
1. 读取 `modules/amemo-save-memo/SKILL.md`
2. 按参数格式构造请求
3. 用 curl 发送 POST 请求到 `https://skill.amemo.cn/save-memo`
