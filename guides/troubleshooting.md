---
title: 故障排除
tags: [guide, troubleshooting, debug]
aliases: [调试指南, 常见问题, FAQ]
date: 2026-05-22
publish: true
---

# 故障排除

本文档列出了使用 Note System 时的常见问题和解决方案。

## 开发环境问题

### 端口冲突

> [!warning] 端口占用
> 如果 8788 或 5173 端口被占用，开发服务器无法启动。

**解决方案**：

```bash
# 查找占用端口的进程
netstat -ano | grep 8788

# 终止进程
taskkill //F //PID <PID>
```

### Wrangler 静态文件不响应

这是 Windows 上 Wrangler 的已知问题。解决方案是使用双服务器模式：

1. 终端 1：`npx wrangler pages dev ./dist --port 8788`
2. 终端 2：`npm run dev`

详细开发流程参考 [[指南/开发工作流]]。

## API 问题

### GitHub API 返回 403

常见原因：

1. **Token 过期** — 重新生成 GitHub Personal Access Token
2. **缺少 User-Agent** — 系统已自动添加，但如果自定义请求需注意
3. **API 限流** — 认证用户 5000 次/小时，KV 缓存可大幅减少请求

> [!tip] 检查限流状态
> ```bash
> curl -s -H "Authorization: Bearer $TOKEN" https://api.github.com/rate_limit
> ```

### KV 缓存未更新

KV 缓存有 TTL，不会立即过期：

| 数据 | TTL |
|------|-----|
| 笔记索引 | 5 分钟 |
| 站点配置 | 1 小时 |
| 搜索结果 | 2 分钟 |

可以删除 `.wrangler/state/` 目录清除本地缓存。

## 链接问题

### Wikilink 显示为断链

检查以下几点：

1. 目标文件是否存在 `.md` 扩展名
2. 路径是否正确（区分大小写）
3. 是否使用了正确的别名

更多链接语法参考 [[指南/Obsidian语法]]。

### 嵌入不显示

- 确保使用 `![[...]]` 语法（带感叹号）
- 图片文件需要在仓库中实际存在

## 构建问题

### TypeScript 编译失败

```bash
npm run typecheck
```

常见原因：
- 缺少依赖：`npm install`
- 类型不匹配：检查 `src/types/index.ts` 是否与 API 响应一致

完整的 API 响应格式参考 [[指南/API参考]]。
