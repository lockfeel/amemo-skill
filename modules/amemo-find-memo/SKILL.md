---
name: amemo-find-memo
description: >
  amemo 查询备忘录模块。当用户需要查看、搜索已有备忘录时使用。
---

# amemo-find-memo — 查询备忘录

## 接口信息

- **路由**: POST http://127.0.0.1:8092/find-memo
- **Bean**: MemoBean
- **Content-Type**: application/json

## 请求参数

> **注意**：服务端要求所有字段必须存在。`userToken` 和 `memoTitle` 必填且有值，其他字段可选但字段必须存在（可传 `null`）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| userToken | str | **是** | 用户登录凭证 |
| memoId | str | 否 | 按 ID 精确查询，不传则传 `null` |
| memoTitle | str | **是** | 按标题模糊查询（不能为空） |
| memoContent | str | 否 | 按内容模糊查询，不传则传 `null` |

## 请求示例

```bash
# 按标题查询
curl -X POST http://127.0.0.1:8092/find-memo \
  -H "Content-Type: application/json" \
  -d '{"userToken": "<token>", "memoId": null, "memoTitle": "量化", "memoContent": null}'
```

## 响应示例

```json
{
  "code": 200,
  "desc": "success",
  "data": {
    "text": "## 相关笔记\n- 2025-11-08 05:14:24\n\n笔记内容...\n- 2012-01-29 09:26:03\n\n笔记内容..."
  }
}
```

## 响应解析

| 字段 | 类型 | 说明 |
|------|------|------|
| code | int | 状态码，200 表示成功 |
| desc | str | 状态描述 |
| data.text | str | Markdown 格式的笔记列表，包含时间和内容 |

## 数据格式说明

返回的 `data.text` 是 Markdown 格式，结构如下：

```markdown
## 相关笔记
- 2025-11-08 05:14:24

笔记内容（支持多行）

- 2012-01-29 09:26:03

笔记内容...
```

每条笔记包含：
- 时间戳（列表项格式）
- 笔记内容（段落格式，支持多行）

## 注意事项

- 只需传入 `userToken` 和 `memoTitle` 即可查询
- 返回的笔记按时间倒序排列
- 内容已格式化为 Markdown，可直接展示给用户
