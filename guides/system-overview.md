---
title: 系统全景
tags: [guide, overview, visual]
aliases: [全景图, 系统总览]
date: 2026-05-22
publish: true
---

# 系统全景

本文通过图示展示 Note System 的完整架构和工作流程。

## 架构总览

系统的整体架构如下，展示了从浏览器到 GitHub API 的完整数据链路：

![[assets/architecture-diagram.svg]]

如 [[指南/架构]] 中所述，所有请求先经过 Cloudflare KV 缓存，未命中时才请求 GitHub API。

> [!note] 缓存保护
> KV 缓存将 GitHub API 调用减少了 90% 以上。详细的 TTL 策略参考 [[指南/性能优化]]。^cache-note

## Obsidian 链接解析流程

当笔记仓库中的 Markdown 文件被索引时，系统执行以下解析流程：

![[assets/obsidian-features.svg]]

主要步骤：

1. **原始 Markdown** — 读取 `.md` 文件内容
2. **链接提取** — 识别 `[[wikilinks]]`、`![[embeds]]`、Markdown 链接
3. **链接解析** — 将原始目标匹配到实际笔记路径
4. **Vault 索引** — 构建完整的笔记关系图
5. **前端渲染** — 将解析后的数据渲染为可视化页面

## 界面预览

实际运行效果如下：

![[assets/ui-screenshot.svg]]

关键界面元素：

- **左侧边栏** — 目录树导航
- **顶部 Header** — 站点标题 + 搜索栏（标题来自 `site.yml`）
- **主内容区** — Markdown 渲染 + 代码高亮 + Callout
- **Backlinks 区域** — 显示引用当前笔记的其他笔记

## 数据流

```
用户请求
  → Vite Dev Server (前端)
    → API 代理 /api/*
      → Wrangler (Cloudflare Functions)
        → KV 缓存检查
          → 命中: 直接返回
          → 未命中: GitHub API → 写入 KV → 返回
```

开发环境搭建详见 [[指南/开发工作流]]，故障排查参考 [[指南/故障排除]]。
