---
title: 更新日志
tags: [changelog, meta]
aliases: [变更记录, 版本历史]
date: 2026-05-22
publish: true
---

# 更新日志

记录 Note System 的版本更新和功能变更。

## v0.2 — Obsidian 兼容 {#v0.2}

_2026-05-22_

### 新增

- Obsidian 风格双向链接 `[[wikilinks]]`
- 笔记嵌入 `![[note]]`
- Callout 语法 `> [!type] Title`
- 标签系统（frontmatter + 内联 `#tag`）
- 反向链接（backlinks）自动计算
- 块引用锚点 `^block-id`
- 标题锚点链接
- 别名（aliases）支持
- Vault 索引和链接关系图
- 附件管理（图片、音频、视频、PDF）
- 链接诊断（断链检测、歧义提示）

### 改进

- 站点配置由笔记仓库 `site.yml` 驱动
- 首页渲染 `README.md` 内容
- 动态标题（header + 浏览器标签页）

## v0.1 — MVP {#v0.1}

_2026-05-21_

### 新增

- Vue 3 + TypeScript + Vite 项目搭建
- Tailwind CSS 4 集成
- Cloudflare Pages Functions API 层
- GitHub Contents API 笔记读取
- Cloudflare KV 边缘缓存
- 目录树导航
- Markdown 渲染（带代码高亮）
- 全文搜索
- 响应式布局（桌面 + 移动端）

> [!note] 下一步
> 参考 [[指南/架构]] 了解未来计划。开发相关问题参考 [[指南/开发工作流]]。

---

本项目的所有指南文章：

- [[指南/Obsidian语法]] — Obsidian 语法支持
- [[指南/架构]] — 系统架构
- [[指南/API参考]] — API 文档
- [[指南/配置]] — 站点配置
- [[指南/开发工作流]] — 开发流程
- [[指南/性能优化]] — 性能优化
- [[指南/安全]] — 安全策略
- [[指南/搜索]] — 搜索功能
- [[指南/故障排除]] — 常见问题
- [[指南/部署]] — 部署指南
