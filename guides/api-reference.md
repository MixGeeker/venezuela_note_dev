---
title: API 参考
tags: [guide, api, reference]
aliases: [API文档, 接口文档]
date: 2026-05-22
publish: true
---

# API 参考

本文档描述 Note System 的所有 API 端点。系统架构概述见 [[指南/架构]]。

## GET /api/notes

获取完整的笔记目录树。

### 响应

```json
[
  {
    "name": "README.md",
    "path": "README.md",
    "type": "file"
  },
  {
    "name": "guides",
    "path": "guides",
    "type": "dir",
    "children": [...]
  }
]
```

## GET /api/notes/:path

获取单篇笔记的完整内容。

### 路径参数

| 参数 | 说明 |
|------|------|
| `path` | 笔记在仓库中的相对路径，如 `guides/architecture.md` |

> [!warning] 路径编码
> 路径中的每个段需要 URL 编码。例如 `guides/obsidian syntax.md` 应编码为 `guides/obsidian%20syntax.md`。

### 响应

```json
{
  "meta": {
    "name": "example.md",
    "path": "example.md",
    "title": "示例笔记",
    "aliases": ["例子"],
    "tags": ["example"],
    "headings": [...],
    "outboundLinks": [...],
    "backlinks": [...]
  },
  "frontmatter": { "title": "示例笔记" },
  "content": "原始 Markdown 内容",
  "html": "原始 Markdown 内容",
  "links": [...],
  "embeds": [...],
  "diagnostics": [...]
}
```

## GET /api/search?q=:query

搜索笔记内容。

### 查询参数

| 参数 | 必填 | 说明 |
|------|------|------|
| `q` | 是 | 搜索关键词 |

## GET /api/config

获取站点配置（来自 `site.yml`）。

> [!tip] 缓存
> 配置请求缓存 1 小时。修改 `site.yml` 后最多需要 1 小时生效。

### 响应

```json
{
  "title": "Venezuela Dev Notes",
  "description": "开发笔记与指南"
}
```

## 错误处理

所有端点遵循统一的错误格式：

```json
{
  "message": "Note not found",
  "status": 404
}
```

| 状态码 | 含义 |
|--------|------|
| 400 | 无效请求 |
| 404 | 资源不存在 |
| 500 | 服务器错误 |

常见问题排查参考 [[指南/故障排除]]。
