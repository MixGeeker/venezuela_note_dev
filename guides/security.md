---
title: 安全
tags: [guide, security]
aliases: [安全指南, 安全策略]
date: 2026-05-22
publish: true
---

# 安全

本文档描述 Note System 的安全设计和最佳实践。

## Token 安全

### GitHub Personal Access Token

系统使用 GitHub PAT 读取笔记仓库内容。Token 安全至关重要：

> [!danger] 危险
> **绝对不要**将 GitHub Token 提交到代码仓库。Token 泄露可能导致仓库被未授权修改。

**最佳实践**：

1. 使用 Fine-grained Token，仅授予 `Contents → Read-only` 权限
2. Token 仅存储在 `.dev.vars`（本地开发）或 Cloudflare Secrets（生产环境）
3. `.dev.vars` 已加入 `.gitignore`
4. 定期轮换 Token

### 配置检查

在 [[指南/配置]] 中确认 `.dev.vars` 不在版本控制中：

```bash
git status  # 确认 .dev.vars 不在列表中
cat .gitignore  # 确认 .dev.vars 已列出
```

## 只读设计

MVP 阶段的系统是**纯只读**的：

- 前端只能通过 API 读取笔记
- API 只调用 GitHub Contents API 的 GET 方法
- 没有写入、删除或修改能力

> [!note] 未来计划
> P2 阶段将引入编辑功能，通过 PR 模型实现。所有修改需要经过 review 才能合并。

## API 安全

### 路径遍历防护

API 端点拒绝包含 `..` 的路径：

```typescript
if (normalizeVaultPath(notePath) === null) {
  return Response.json({ message: 'Invalid path', status: 400 })
}
```

### 请求头

所有 GitHub API 请求包含：
- `Authorization` — Token 认证
- `User-Agent` — 标识请求来源
- `Accept` — 指定响应格式

## Cloudflare 安全

### KV 缓存

- KV 数据仅存储在 Cloudflare 边缘节点
- Token 不存储在 KV 中
- 缓存数据不包含敏感信息

### 环境变量

生产环境使用 Cloudflare Secrets（加密存储），而非明文环境变量。

## 检查清单

- [ ] `.dev.vars` 在 `.gitignore` 中
- [ ] Token 权限仅限 `Contents → Read-only`
- [ ] `wrangler.toml` 不包含密钥
- [ ] `_routes.json` 限制 Functions 仅处理 `/api/*`

更多排查参考 [[指南/故障排除]]。架构设计见 [[指南/架构]]。
