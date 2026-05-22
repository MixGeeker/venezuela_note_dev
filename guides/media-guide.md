---
title: 多媒体笔记指南
tags: [guide, media, images, attachments]
aliases: [图片指南, 附件使用]
date: 2026-05-22
publish: true
---

# 多媒体笔记指南

Note System 支持在笔记中嵌入图片、音频、视频和 PDF 等附件。本文演示如何使用这些功能。

## 嵌入图片

### 使用 Wikilink 嵌入

最简单的方式是使用 `![[图片路径]]` 语法：

![[assets/ui-screenshot.svg]]

### 带尺寸的嵌入

可以使用 `![[图片路径|宽度x高度]]` 语法控制显示尺寸：

![[assets/obsidian-features.svg|500x360]]

> [!tip] 图片路径
> 图片路径相对于笔记仓库根目录。将图片放在 `assets/` 目录下是推荐的组织方式。

## 架构图

在技术文档中，架构图是不可或缺的：

![[assets/architecture-diagram.svg]]

这张架构图展示了系统的核心数据流。更多信息参考 [[指南/系统全景]]。

## 支持的附件类型

| 类型 | 扩展名 | 嵌入方式 |
|------|--------|----------|
| 图片 | `.png` `.jpg` `.gif` `.svg` `.webp` | 内联显示 |
| 音频 | `.mp3` `.wav` `.ogg` | 播放器控件 |
| 视频 | `.mp4` `.webm` `.mov` | 视频播放器 |
| PDF | `.pdf` | iframe 嵌入 |

## Markdown 图片语法

除了 Wikilink 嵌入，也支持标准 Markdown 图片语法：

```markdown
![描述文字](图片路径)
```

> [!warning] 注意
> Markdown 图片语法中的路径不支持自动附件解析，推荐使用 `![[]]` 语法。

## 最佳实践

1. **统一存放** — 所有附件放在 `assets/` 目录下
2. **语义命名** — 使用有意义的文件名，如 `architecture-diagram.svg`
3. **适当尺寸** — 图片宽度控制在 800px 以内
4. **SVG 优先** — 对于图表和示意图，SVG 格式最适合

## 相关链接

- [[指南/系统全景]] — 包含多张架构图的完整系统介绍
- [[指南/Obsidian语法]] — 所有 Obsidian 语法特性
- [[指南/架构]] — 系统架构详解 ^related-links
