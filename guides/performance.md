---
title: 性能优化
tags: [guide, performance, optimization]
aliases: [优化指南, 性能]
date: 2026-05-22
publish: true
---

# 性能优化

Note System 通过多层缓存和优化策略确保快速响应。

## 缓存架构

系统使用 Cloudflare KV 作为边缘缓存层，大幅减少对 GitHub API 的直接请求。

```
请求 → Cloudflare KV 缓存 → 命中 → 直接返回
                          → 未命中 → GitHub API → 写入缓存 → 返回
```

## GitHub API 限流

| 认证方式 | 请求限制 |
|----------|----------|
| 无 Token | 60 次/小时 |
| 有 Token | 5,000 次/小时 |

> [!warning] 限流风险
> 虽然认证用户有 5000 次/小时的配额，但在高并发场景下仍然可能触发限流。KV 缓存是主要保护机制。

## 缓存 TTL 策略

| 数据类型 | TTL | 理由 |
|----------|-----|------|
| Vault 索引 | 5 分钟 | 笔记结构偶尔变化 |
| 站点配置 | 1 小时 | 几乎不变 |
| 搜索结果 | 2 分钟 | 需要较新的结果 |
| 单篇笔记 | 5 分钟 | 内容更新频率适中 |

详细的缓存设计见 [[指南/架构]]。^cache-strategy

## 前端优化

### 代码分割

路由级别的懒加载已经实现：

```typescript
component: () => import('./pages/NotePage.vue')
```

> [!note] highlight.js 包体积
> `NoteViewer` 组件包含 highlight.js，导致该 chunk 超过 1MB。未来可以考虑按需加载语言包。

### Markdown 渲染

Markdown 渲染在前端执行，避免后端引入重量级依赖。渲染结果通过 `computed` 缓存，笔记内容不变时不会重复渲染。

## 后端优化

### Vault 索引

系统在首次请求时构建完整的 Vault 索引，包含：

- 所有笔记的元数据、标题、标签、别名
- 链接关系图（outbound + backlinks）
- 嵌入和附件信息

索引一旦构建就缓存 5 分钟，后续请求直接返回缓存结果。

### Obsidian 链接解析

链接解析在索引构建时一次性完成，不需要每次请求都重新解析。详见 [[指南/Obsidian语法]] 和 [[指南/API参考]]。

## 未来优化方向

1. **增量索引更新** — 通过 GitHub webhook 触发缓存失效
2. **流式渲染** — 大文档分段加载
3. **Service Worker** — 离线缓存支持

开发流程参考 [[指南/开发工作流]]。
