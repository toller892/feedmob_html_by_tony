# fm-service-deployer 完整使用指南

## 架构概览

AI Agent → MCP Server (`3.15.32.167:30002`, SSE) → Backend API (`127.0.0.1:30001`) → PostgreSQL

---

## 1. Agent 接入 MCP

在 Claude Code 的 `~/.claude.json` 或项目 `.mcp.json` 中添加：

```json
{
  "mcpServers": {
    "page-publisher": {
      "type": "sse",
      "url": "http://3.15.32.167:30002/sse",
      "headers": {
        "Authorization": "Bearer GSzM9g31cL-lOvVn7xxyu-NGlrjT4pCDR83jtw0aUvo"
      }
    }
  }
}
```

---

## 2. 发布 HTML 页面

调用 MCP tool `publish_page`：

| 参数 | 必填 | 说明 |
|---|---|---|
| `html` | 是 | 完整 HTML 内容 |
| `owner` | 是 | 归属组（小写字母、数字、连字符） |
| `submitter` | 是 | 提交者名称或邮箱 |
| `description` | 是 | 页面简述 |
| `slug` | 否 | URL 标识，不填则自动生成 10 位 |
| `title` | 否 | 页面标题 |

示例（Agent 自然语言即可触发）：

> "帮我把这份 HTML 发布到 feedmob，owner 设成 demo，submitter 填 tony"

---

## 3. 访问已发布页面

发布成功后，浏览器打开：

```
http://3.15.32.167:30001/p/{owner}/{slug}?token=GSzM9g31cL-lOvVn7xxyu-NGlrjT4pCDR83jtw0aUvo
```

---

## 4. 查看页面列表与统计

### 管理控制台（推荐）

```
http://3.15.32.167:30001/console?token=GSzM9g31cL-lOvVn7xxyu-NGlrjT4pCDR83jtw0aUvo
```

### 通过 MCP Tool

| Tool | 用途 |
|---|---|
| `list_pages` | 分页列表搜索，参数 `page`, `page_size`, `q`（关键词搜索） |
| `get_page_stats` | 统计数据：总数、top owner、top submitter |
| `get_page_detail` | 单个页面元数据，参数 `owner`, `slug` |

---

## 5. 鉴权

- URL 参数方式：`?token=GSzM9g31cL-lOvVn7xxyu-NGlrjT4pCDR83jtw0aUvo`
- Header 方式：`Authorization: Bearer GSzM9g31cL-lOvVn7xxyu-NGlrjT4pCDR83jtw0aUvo`
- `/health` 与 `/assets/*` 无需鉴权
