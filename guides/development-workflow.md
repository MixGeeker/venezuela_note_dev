---
title: 开发工作流
tags: [guide, workflow, dev]
aliases: [开发流程, 贡献指南]
date: 2026-05-22
publish: true
---

# 开发工作流

本文档描述 Note System 的本地开发环境搭建和日常工作流。

## 环境准备

### 前置要求

- Node.js 18+
- npm
- Git
- GitHub 账号和 Personal Access Token

### 初始化项目

```bash
git clone https://github.com/MixGeeker/venezuela_note.git
cd venezuela_note
npm install
```

## 双服务器开发模式

由于 Wrangler 在 Windows 上的静态文件服务有限制，开发时使用双服务器模式：

**终端 1 — API 服务**：

```bash
npm run build
npx wrangler pages dev ./dist --port 8788
```

**终端 2 — 前端热更新**：

```bash
npm run dev
```

> [!important] 端口说明
> - `8788`：Wrangler API 服务（Cloudflare Functions + KV）
> - `5173`：Vite 前端（自动代理 `/api` 到 8788）
>
> 浏览器访问 `http://localhost:5173`

Vite 配置了 API 代理，所有 `/api/*` 请求自动转发到 Wrangler：

```typescript
// vite.config.ts
server: {
  proxy: { '/api': 'http://127.0.0.1:8788' }
}
```

## Token 配置

在 `.dev.vars` 文件中配置 GitHub Token：

```
GITHUB_TOKEN=ghp_your_token_here
```

> [!warning] 安全提醒
> `.dev.vars` 已加入 `.gitignore`，切勿提交 Token 到仓库。详见 [[指南/安全]]。

## 常用命令

| 命令 | 用途 |
|------|------|
| `npm run dev` | 启动 Vite 开发服务器 |
| `npm run build` | 类型检查 + 构建 |
| `npm run typecheck` | 仅类型检查 |
| `npm run preview` | 预览构建产物 |

## 修改笔记仓库

笔记内容在独立的仓库中管理。详见 [[指南/配置]] 了解仓库设置。

## 问题排查

遇到问题时参考 [[指南/故障排除]]。^troubleshoot-ref
