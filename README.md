# amemo-skill

专为 AI 工具链接麦小记 APP 而开发的技能包，专注于笔记、清单和健康数据的管理。

支持 OpenClaw、Claude Code、Codex、OpenCode 等 AI 工具。

## 功能特性

- **笔记管理** - 保存和查询笔记，自动整理对话内容
- **清单管理** - 创建待办任务，支持邮件提醒
- **健康数据** - 查询步数、睡眠、血氧、血压、心率、消耗等健康数据
- **健康简报** - 一键生成今日健康数据汇总
- **AI记忆** - 同步 AI 助手记忆（仅限 OpenClaw 系列应用）

## 安装说明

### 通过SKILL市场安装

1. 先安装 clawhub 技能管理器：

```bash
npx clawhub@latest install amemo-skill
```

1. 通过 find-skills 安装 amemo-skill：

```bash
find-skills install amemo-skill
```

### 手动安装GIT安装

#### Claude Code

```bash
git clone https://github.com/lockfeel/amemo-skill.git ~/.claude/skills/amemo-skill
rm -rf ~/.claude/skills/amemo-skill/.git
rm -rf ~/.claude/skills/amemo-skill/.claude
```

### Codex

```bash
git clone https://github.com/lockfeel/amemo-skill.git ~/.codex/skills/amemo-skill
rm -rf ~/.codex/skills/amemo-skill/.git
rm -rf ~/.codex/skills/amemo-skill/.claude
```

### OpenCode

```bash
git clone https://github.com/lockfeel/amemo-skill.git ~/.opencode/skills/amemo-skill
rm -rf ~/.opencode/skills/amemo-skill/.git
rm -rf ~/.opencode/skills/amemo-skill/.claude
```

### OpenClaw

```bash
git clone https://github.com/lockfeel/amemo-skill.git ~/.openclaw/skills/amemo-skill
rm -rf ~/.openclaw/skills/amemo-skill/.git
rm -rf ~/.openclaw/skills/amemo-skill/.claude
```

### 其他Claw

```code
请帮我从这个仓库：https://github.com/lockfeel/amemo-skill.git 下载，安装这个SKILL
```

复制以上仓库路径，直接发送给AI对话中，

安装后重启 AI 工具即可生效。

amemo 服务为远程线上服务，默认地址：`https://api.amemo.com`

## 快速开始

### 登录

首次使用需要登录：

```text
发送你的手机号：13800138000
```

系统会自动发送验证码，输入验证码完成登录。

### 使用示例

**保存笔记：**

```text
帮我记一下这个重要的知识点
```

**查询笔记：**

```text
查看我 Python 相关的笔记
```

**创建任务：**

```text
明天要去开会讨论项目进度
```

**查看健康数据：**

```text
查看我的步数数据
今日健康简报
```

## 调度模块

| 模块 | 功能 |
|------|------|
| `amemo-login` | 用户登录 |
| `amemo-send-code` | 发送验证码 |
| `amemo-save-memo` | 保存笔记 |
| `amemo-find-memo` | 查询笔记 |
| `amemo-save-task` | 创建任务 |
| `amemo-find-task` | 查询任务 |
| `amemo-send-task` | 发送任务（邮件提醒） |
| `amemo-find-data` | 查询健康数据 |
| `amemo-last-data` | 健康简报 |
| `amemo-init-mate` | 刷新 AI 记忆 |
| `amemo-save-mate` | 保存 AI 记忆 |

## 触发词

### 笔记

- 保存笔记、记下这一条、记录笔记、帮我记一下、保存备忘
- 查看我 XXX 相关的笔记、查找 XXX 相关的笔记、搜索我 XXX 相关的笔记

### 任务

- 今天 XXX、明天 XXX、昨天 XXX、后天 XXX
- 将来 XXX，未来 XXX、最近 XXX、最新 XXX、近期 XXX
- 12月XX日 XXX（具体日期）
- 查看我的清单、查询清单、我的待办、查看任务

### 健康数据

- 查看我的 XXX 数据（步数/睡眠/血氧/血压/心率/消耗）
- XXX 数据怎么样
- 今日健康简报、今日健康总览、健康简报、健康日报

### AI 记忆

- 刷新助手记忆、初始化助手记忆、重置记忆
- 保存永久记忆、永久记住 XXX、记住这个

## 注意事项

- AI 记忆模块（init-mate、save-mate）仅适用于 OpenClaw 或基于 OpenClaw 开发的应用
- 除登录/验证码外，所有接口需要先完成登录获取 userToken
- 邮件提醒需要在创建任务时提供有效的邮箱地址
