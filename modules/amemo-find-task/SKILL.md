---
name: amemo-find-task
description: >
  amemo 查询任务模块。当用户需要查看、搜索已有任务时使用。
---

# amemo-find-task — 查询任务

## 接口信息

- **路由**: POST https://skill.amemo.cn/find-task
- **Bean**: TaskBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken` 必填且有值，其他字段可选但字段必须存在（可传 `null`）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证 |
| taskId | str | 否 | 按 ID 精确查询，不传则传 `null` |
| taskTitle | str | 否 | 按标题模糊查询，不传则传 `null` |
| taskTime | str | 否 | 按时间筛选，不传则传 `null` |
| taskEmail | list[str] | 否 | 按邮箱筛选，不传则传 `null` |

## 请求示例

```bash
# 查询所有任务（所有可选字段传 null）
curl -X POST https://skill.amemo.cn/find-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskId": null, "taskTitle": null, "taskTime": null, "taskEmail": null}'

# 按标题查询
curl -X POST https://skill.amemo.cn/find-task \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "taskId": null, "taskTitle": "报告", "taskTime": null, "taskEmail": null}'
```

## 响应数据结构

```json
{
  "code": 200,
  "desc": "success",
  "data": {
    "recommend": [],           // 今日推荐列表
    "myFollow": [],           // 我的收藏列表
    "todayList": [],         // 今日列表
    "tomorrowList": [],       // 明日列表
    "recentList": [],         // 近期列表（近15天）
    "finishList": [],         // 已完成列表
    "futureList": []          // 未来列表
  }
}
```

### TaskInfo 任务信息

| 字段 | 类型 | 说明 |
|------|------|------|
| taskId | str | 任务唯一标识 |
| taskTitle | str | 任务标题 |
| recentRemindTime | int | 最近的提醒时间 |

### MyFollowTask 收藏任务信息

| 字段 | 类型 | 说明 |
|------|------|------|
| taskId | str | 任务唯一标识 |
| taskTitle | str | 任务标题 |
| recentRemindTime | int | 最近的提醒时间 |

## 待办清单展示模板

调用成功后，根据返回数据为用户组织清晰的任务清单展示。

### 无数据时回复

```
暂无待办清单。

你可以：
• 创建新的待办任务
• 使用「保存任务」添加待办
```

### 任务清单展示模板

````markdown
## ✅ 待办清单

---

### 📅 今日待办 {todayCount} 项
{today_list_items}

---

### 📅 明日待办 {tomorrowCount} 项
{tomorrow_list_items}

---

### 📆 近期待办（15天内）{recentCount} 项
{recent_list_items}

---

### 📋 未来待办 {futureCount} 项
{future_list_items}

---

### ⭐ 我的收藏 {followCount} 项
{follow_list_items}

---

### ✔ 已完成 {finishCount} 项
{finish_list_items}
````

### 单个任务项格式化

**待做任务格式：**
```
{index}. {taskTitle}
```

**收藏任务格式：**
```
{index}. ⭐ {taskTitle}
```

**已完成任务格式：**
```
{index}. ✔ {taskTitle}
```

### 任务状态图标

| 状态 | 图标 | 说明 |
|------|------|------|
| pending | ⏳ | 待完成 |
| completed | ✔ | 已完成 |
| expired | ❌ | 已过期 |
| follow | ⭐ | 已收藏 |

### 分组展示优先级

1. **今日待办** — 今日必须完成的任务，优先展示
2. **明日待办** — 明日计划的任务
3. **近期待办** — 未来15天内的任务
4. **未来待办** — 15天之后的任务
5. **我的收藏** — 用户收藏的重要任务
6. **已完成** — 已完成的任务（可折叠显示）

### 分类计数统计

```markdown
| 分类 | 数量 |
|------|------|
| 今日待办 | {count} |
| 明日待办 | {count} |
| 近期待办 | {count} |
| 未来待办 | {count} |
| 我的收藏 | {count} |
| 已完成 | {count} |
| **总计** | **{count}** |
```

### 任务为空时处理

如果某分类为空：
- 今日/明日/近期为空：显示 `暂无15天内待办`
- 未来为空：显示 `暂无未来待办`
- 收藏为空：显示 `暂无收藏任务`
- 已完成为空：显示 `暂无已完成任务`

### 输出示例

```markdown
## ✅ 待办清单

---

### 📅 今日待办 3 项
1. 完成项目报告
2. 提交代码审查
3. 准备会议材料

---

### 📅 明日待办 2 项
1. 产品需求评审
2. 团队周会

---

### 📆 近期待办（15天内） 4 项
1. 客户方案调整
2. 技术文档更新
3. 测试报告评审
4. 版本发布准备

---

### 📋 未来待办 2 项
1. 季度 OKR 制定
2. 年度总结规划

---

### ⭐ 我的收藏 1 项
1. ⭐ 年度总结

---

### ✔ 已完成 5 项
1. ✔ 登录功能开发
2. ✔ 数据库优化
3. ✔ API 接口调试
4. ✔ 前端页面联调
5. ✔ 部署文档编写

---

### 📊 统计概览

| 分类 | 数量 |
|------|------|
| 今日待办 | 3 |
| 明日待办 | 2 |
| 近期待办 | 4 |
| 未来待办 | 2 |
| 我的收藏 | 1 |
| 已完成 | 5 |
| **总计** | **17** |
```

## 调用示例

**用户输入：**
```
查看我的待办清单
```

**系统处理：**
1. 检查 `userToken`
2. 调用：`POST /find-task` with `userToken: "{token}"`
3. 解析返回数据，按分类组织展示

**用户输入：**
```
查找我的清单
```

**系统处理：**
1. 检查 `userToken`
2. 调用：`POST /find-task` with `userToken: "{token}"`
3. 格式化输出给用户
