---
title: 站点配置
tags: [guide, config]
aliases: [配置指南, site.yml]
date: 2026-05-22
publish: true
---

# 站点配置

Note System 的外观和行为由笔记仓库根目录的 `site.yml` 文件控制。

## site.yml 格式

```yaml
# 站点标题（显示在 header 和浏览器标签页）
title: My Notes

# 站点描述
description: 我的个人笔记库
```

## 配置项说明

### title

- **必填**：否
- **默认值**：`"Notes"`
- **作用**：显示在页面顶部 Header 和浏览器标签页标题

### description

- **必填**：否
- **默认值**：`""`
- **作用**：站点描述，未来可用于 SEO

> [!note] 可选文件
> `site.yml` 是完全可选的。如果仓库中不存在此文件，系统使用默认值。参考 [[指南/Obsidian语法]] 了解更多 Obsidian 特性。

## 环境变量

在 `wrangler.toml` 中配置的变量控制后端行为：

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `GITHUB_OWNER` | GitHub 仓库所有者 | — |
| `GITHUB_REPO` | 笔记仓库名 | — |
| `NOTES_PATH` | 仓库内笔记子目录 | `""`（根目录） |

`GITHUB_TOKEN` 作为密钥配置在 `.dev.vars` 中，不应提交到仓库。安全相关事项参考 [[指南/安全]]。

## 首页配置

系统自动从笔记仓库根目录读取 `README.md` 作为首页内容。如果 `README.md` 不存在，则显示文件列表。

系统架构详见 [[指南/架构]]。
