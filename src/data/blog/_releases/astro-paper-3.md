---
author: Sat Naing
pubDatetime: 2023-09-25T10:25:54.547Z
title: AstroPaper 3.0
slug: astro-paper-v3
featured: false
ogImage: https://github.com/satnaing/astro-paper/assets/53733092/1ef0cf03-8137-4d67-ac81-84a032119e3a
tags:
  - release
description: "AstroPaper Version 3: Elevating Your Web Experience with Astro v3 and Seamless View Transitions"
---

我们很高兴宣布 AstroPaper v3 的发布，它包含了新功能、增强改进和 Bug 修复，旨在提升你的 Web 开发体验。让我们深入了解本次发布的亮点：

![AstroPaper v3](@/assets/images/AstroPaper-v3.png)

## 目录

## 功能与变更

### Astro v3 集成

<video autoplay loop="loop" muted="muted" plays-inline="true">
  <source src="https://github.com/satnaing/astro-paper/assets/53733092/18fdb604-1ca3-41a0-8372-1367759091ff" type="video/mp4">
  <!-- <source src="/assets/docs/astro-paper-v3-view-transitions-demo.mp4" type="video/mp4"> -->
</video>

AstroPaper 现已全面支持 [Astro v3](https://astro.build/blog/astro-3/)，提供更优的性能和渲染速度。

此外，我们还添加了对 Astro 的 [ViewTransitions API](https://docs.astro.build/en/guides/view-transitions/) 的支持，让你能够在页面视图之间创建迷人且动态的过渡效果。

在「Recent Section」（最近文章板块）中，仅显示非精选文章，以避免重复并更好地支持 ViewTransitions API。

### 更新 OG 图片生成逻辑

![Example OG Image](https://user-images.githubusercontent.com/40914272/269252964-a0dc6735-80f7-41ed-8e74-4d4d70f96891.png)

我们更新了自动 OG 图片生成的逻辑，使其更加可靠和高效。此外，现在支持文章标题中的特殊字符，确保社交媒体预览准确、灵活且引人注目。

`SITE.ogImage` 现在是可选的。如果未指定，AstroPaper 将使用 `SITE.title`、`SITE.desc` 和 `SITE.website` 自动生成 OG 图片。

### Theme 元标签

添加了 theme-color 元标签，以动态适应主题切换，确保流畅的用户体验。

> 注意顶部的差异

**_AstroPaper v2 主题切换_**

<video autoplay loop="loop" muted="muted" plays-inline="true">
  <source src="https://github.com/satnaing/astro-paper/assets/53733092/3ab5a1e8-1891-4264-a5bb-0ded69143c1a" type="video/mp4">
</video>

**_AstroPaper v3 主题切换_**

<video autoplay loop="loop" muted="muted" plays-inline="true">
  <source src="https://github.com/satnaing/astro-paper/assets/53733092/8ac9deb8-d1f8-4029-86bd-6aa0def380b4" type="video/mp4">
</video>

## 其他变更

### Astro Prettier 插件

Astro Prettier 插件已开箱安装，以保持项目整洁有序。

### 样式微调

单行代码块换行问题已解决，让你的代码片段看起来整洁清新。

更新了导航样式 CSS，允许在导航栏中添加更多导航链接。

## 升级到 AstroPaper v3

> 本节仅适用于想要从旧版本升级到 AstroPaper v3 的用户。

本节将帮助你从 AstroPaper v2 迁移到 AstroPaper v3。

在阅读本节其余内容之前，你可能还想查看[这篇文章](https://astro-paper.pages.dev/posts/how-to-update-dependencies/)了解如何升级依赖项和 AstroPaper。

## 方案一：全新重启（推荐）

本次发布中进行了大量更改——用新的 Astro API 替换旧的 API、Bug 修复、新功能等。因此，如果你没有进行过多自定义，建议采用此方案。

**_步骤 1：保留所有已更新的文件_**

保留所有已经更新过的文件非常重要。这些文件包括：

- `/src/config.ts`（v3 中未改动）
- `/src/styles/base.css`（v3 中有微小改动；详见下文）
- `/src/assets/`（v3 中未改动）
- `/public/assets/`（v3 中未改动）
- `/content/blog/`（这是你的博客内容目录 🤷🏻‍♂️）
- 你做的任何其他自定义修改。

```css
/* file: /src/styles/base.css */
@layer base {
  /* 其他代码 */
  ::-webkit-scrollbar-thumb:hover {
    @apply bg-skin-card-muted;
  }

  /* 旧代码
  code {
    white-space: pre;
    overflow: scroll;
  } 
  */

  /* 新代码 */
  code,
  blockquote {
    word-wrap: break-word;
  }
  pre > code {
    white-space: pre;
  }
}

@layer components {
  /* 其他代码 */
}
```

**_步骤 2：用 AstroPaper v3 替换其他所有内容_**

在此步骤中，将除上述文件/目录（以及你自定义的文件/目录）以外的所有内容替换为 AstroPaper v3。

**_步骤 3：Schema 更新_**

请注意，`/src/content/_schemas.ts` 已被替换为 `/src/content/config.ts`。

此外，`/src/content/config.ts` 不再导出 `BlogFrontmatter` 类型。

因此，所有文件中的 `BlogFrontmatter` 类型都需要更新为 `CollectionEntry<"blog">["data"]`。

例如：`src/components/Card.tsx`

```ts
// AstroPaper v2
import type { BlogFrontmatter } from "@content/_schemas";

export interface Props {
  href?: string;
  frontmatter: BlogFrontmatter;
  secHeading?: boolean;
}
```

```ts
// AstroPaper v3
import type { CollectionEntry } from "astro:content";

export interface Props {
  href?: string;
  frontmatter: CollectionEntry<"blog">["data"];
  secHeading?: boolean;
}
```

## 方案二：使用 Git 升级

此方案不推荐大多数用户使用。如果可以，请选择「方案一」。只有在你懂得如何解决合并冲突且清楚自己在做什么的情况下才采用此方案。

实际上，我已经为此写了一篇博客文章，你可以在[这里](https://astro-paper.pages.dev/posts/how-to-update-dependencies/#updating-astropaper-using-git)查看。

## 结语

准备好探索 AstroPaper v3 中令人兴奋的新功能和改进了吗？立即开始[使用 AstroPaper](https://github.com/satnaing/astro-paper)。

有关其他 Bug 修复和集成更新，请查看[发布说明](https://github.com/satnaing/astro-paper/releases/tag/v3.0.0)了解更多。

如果你在升级过程中遇到任何 Bug 或困难，请随时在 [GitHub](https://github.com/satnaing/astro-paper) 上提交 Issue 或发起讨论。
