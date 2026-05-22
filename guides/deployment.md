---
title: 部署指南
tags: [guide, deploy]
date: 2026-05-21
---

# 部署指南

## Cloudflare Pages 部署

1. 构建: `npm run build`
2. 部署: `npx wrangler pages deploy dist`
3. 配置环境变量

## 环境变量

- `GITHUB_TOKEN` — GitHub API 访问令牌
- `GITHUB_OWNER` — 仓库所有者
- `GITHUB_REPO` — 笔记仓库名
- `NOTES_PATH` — 笔记目录路径
