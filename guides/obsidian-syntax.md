---
title: Obsidian 语法支持
tags: [guide, syntax, obsidian]
aliases: [语法指南, markdown语法]
date: 2026-05-22
publish: true
---

# Obsidian 语法支持

本系统完整支持 Obsidian 风格的笔记语法。这篇文章展示了所有支持的特性。

## 双向链接

最基本的 Obsidian 特性是双向链接：

- 使用 `[[指南/架构]]` 链接到其他笔记
- 可以用 `[[指南/架构|系统架构]]` 自定义显示文本
- 链接会在目标笔记中自动生成反向链接

> [!tip] 提示
> 点击侧栏的目录树，或者在搜索框中输入关键词，可以快速找到你想要链接的笔记。

## 嵌入

使用 `![[...]]` 语法可以嵌入其他笔记的内容：

![[changelog#v0.1]]

也可以嵌入特定标题下的内容。

## 标签

在 frontmatter 中定义标签：

```yaml
tags: [guide, syntax]
```

或者使用内联标签：#obsidian #语法指南

## Callout 语法

> [!note] 笔记
> 这是一段笔记类型的 callout。

> [!warning] 警告
> 请注意，某些高级功能可能需要特定配置。参考 [[指南/配置]]。

> [!tip] 小技巧
> 使用 `#^block-id` 可以创建块引用锚点。^obsidian-tip

> [!important] 重要
> 系统架构相关内容请参考 [[指南/架构]]。

## 块引用

在行尾添加 `^block-id` 可以创建可引用的块：^example-block

然后使用 `[[guides/obsidian-syntax#^example-block]]` 来引用这个块。

## 代码块

```typescript
import { defineStore } from 'pinia'

export const useNotesStore = defineStore('notes', () => {
  const tree = ref<TreeNode[]>([])
  return { tree }
})
```

## 表格

| 语法 | 示例 | 说明 |
|------|------|------|
| Wikilink | `[[note]]` | 双向链接 |
| Embed | `![[note]]` | 嵌入内容 |
| Callout | `> [!type]` | 提示框 |
| Tag | `#tag` | 标签 |

## 更多信息

- [[指南/架构]] — 了解系统整体设计
- [[指南/API参考]] — 查看 API 文档
- [[指南/搜索]] — 了解搜索功能
