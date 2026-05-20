---
author: Sat Naing
pubDatetime: 2023-01-30T15:57:52.737Z
title: AstroPaper 2.0
slug: astro-paper-2
featured: false
ogImage: https://user-images.githubusercontent.com/53733092/215771435-25408246-2309-4f8b-a781-1f3d93bdf0ec.png
tags:
  - release
description: AstroPaper with the enhancements of Astro v2. Type-safe markdown contents, bug fixes and better dev experience etc.
---

Astro 2.0 已发布，带来了一些很酷的功能、破坏性变更、开发者体验改进、更好的错误覆盖层等。AstroPaper 充分利用了这些很酷的功能，尤其是 Content Collections API。

<!-- ![Introducing AstroPaper 2.0](https://user-images.githubusercontent.com/53733092/215683840-dc2502f5-8c5a-44f0-a26c-4e7180455056.png) -->

![Introducing AstroPaper 2.0](https://user-images.githubusercontent.com/53733092/215771435-25408246-2309-4f8b-a781-1f3d93bdf0ec.png)

## 目录

## 功能与变更

### 类型安全的 Frontmatter 和重新定义的博客 Schema

得益于 Astro 的 Content Collections，AstroPaper 2.0 的 Markdown 内容 Frontmatter 现在是类型安全的。博客 Schema 定义在 `src/content/_schemas.ts` 文件中。

### 博客内容的新归宿

所有博客文章已从 `src/contents` 目录移动到了 `src/content/blog` 目录。

### 新的获取 API

内容现在使用 `getCollection` 函数获取。不再需要指定内容的相对路径。

```ts
// 旧的内容获取方法
- const postImportResult = import.meta.glob<MarkdownInstance<Frontmatter>>(
  "../contents/**/**/*.md",);

// 新的内容获取方法
+ const postImportResult = await getCollection("blog");
```

### 修改搜索逻辑以获得更好的搜索结果

在旧版 AstroPaper 中，当有人搜索某篇文章时，被搜索的匹配关键词是 `title`、`description` 和 `headings`（headings 指的是博客文章中的所有 h1 ~ h6 标题）。在 AstroPaper v2 中，用户输入时只会搜索 `title` 和 `description`。

### 重命名的 Frontmatter 属性

以下 Frontmatter 属性已被重命名。

| 旧名称 | 新名称 |
| --------- | ----------- |
| datetime | pubDatetime |
| slug | postSlug |

### 博客文章的默认标签

如果一篇博客文章没有任何标签（换句话说，frontmatter 属性 `tags` 未指定），该文章将使用默认标签 `others`。但你可以在 `/src/content/_schemas.ts` 文件中设置默认标签。

```ts
// src/contents/_schemas.ts
export const blogSchema = z.object({
  // ---
  // 将 "others" 替换为你想要的任何内容
  tags: z.array(z.string()).default(["others"]),
  ogImage: z.string().optional(),
  description: z.string(),
});
```

### 新的预定义暗色配色方案

AstroPaper v2 有一套新的暗色配色方案（高对比度和低对比度），基于 Astro 的暗色 Logo。更多信息请查看[此链接](https://astro-paper.pages.dev/posts/predefined-color-schemes#astro-dark)。

![New Predefined Dark Color Scheme](https://user-images.githubusercontent.com/53733092/215680520-59427bb0-f4cb-48c0-bccc-f182a428d72d.svg)

### 自动类名排序

AstroPaper 2.0 包含了使用 [TailwindCSS Prettier 插件](https://tailwindcss.com/blog/automatic-class-sorting-with-prettier) 的自动类名排序功能。

### 更新的文档和 README

所有 [#docs](https://astro-paper.pages.dev/tags/docs/) 博客文章和 [README](https://github.com/satnaing/astro-paper#readme) 均已针对此 AstroPaper v2 进行了更新。

## Bug 修复

- 修复了博客文章页面中损坏的标签
- 在标签页面中，面包屑的最后一部分现在更新为小写以保持一致性
- 在标签页面中排除草稿文章
- 修复了页面重新加载后 'onChange 值不更新' 的问题
