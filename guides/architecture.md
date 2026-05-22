---
title: 系统架构
tags: [guide, architecture]
aliases: [架构设计, 技术架构]
date: 2026-05-22
publish: true
---

# 系统架构

Note System 采用前后端分离的架构，部署在 Cloudflare Pages 上，笔记存储在 GitHub 仓库中。

## 整体架构

```
浏览器 ──→ Cloudflare Pages
              ├── 静态资源 (Vue 3 SPA)
              └── Functions (API)
                    ├── GitHub Contents API
                    └── Cloudflare KV (缓存)
```

## 技术栈

### 前端

- **Vue 3** — Composition API + `<script setup>`
- **TypeScript** — 全栈类型安全
- **Vite** — 构建工具和开发服务器
- **Tailwind CSS 4** — 原子化样式
- **Pinia** — 状态管理
- **markdown-it** — Markdown 渲染

### 后端

- **Cloudflare Pages Functions** — 无服务器 API
- **GitHub Contents API** — 笔记数据源
- **Cloudflare KV** — 边缘缓存

> [!note] 数据流
> 前端通过 API 代理请求后端，后端从 GitHub 读取笔记并通过 KV 缓存结果。

## 缓存策略

| 数据类型 | KV 缓存 Key | TTL |
|----------|-------------|-----|
| Vault 索引 | `vault:index:v1` | 5 分钟 |
| 站点配置 | `config:site` | 1 小时 |
| 搜索结果 | 按查询缓存 | 2 分钟 |

详细配置说明参考 [[指南/配置]]。

## API 端点

完整的 API 文档请参考 [[指南/API参考]]。核心端点包括：

1. `GET /api/notes` — 目录树
2. `GET /api/notes/:path` — 单篇笔记
3. `GET /api/search?q=` — 搜索
4. `GET /api/config` — 站点配置

## Obsidian 兼容层

系统实现了完整的 Obsidian 笔记解析：

- **链接解析** — `[[wikilinks]]` 双向链接和 Markdown 链接
- **嵌入支持** — `![[note]]` 和 `![[image.png]]`
- **Callout** — `> [!type]` 提示框语法
- **标签系统** — frontmatter 标签 + 内联标签
- **反向链接** — 自动计算 backlinks
- **锚点引用** — `#heading` 和 `#^block-id`

详见 [[指南/Obsidian语法]]。^obsidian-compat

## 性能考量

缓存策略保护系统免受 GitHub API 限流影响。更多优化方案参考 [[指南/性能优化]]。

## 安全

安全相关内容参考 [[指南/安全]]。
