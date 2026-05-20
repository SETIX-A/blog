---
author: Sat Naing
pubDatetime: 2022-12-28T04:59:04.866Z
modDatetime: 2025-03-12T13:39:20.763Z
title: Dynamic OG image generation in AstroPaper blog posts
slug: dynamic-og-image-generation-in-astropaper-blog-posts
featured: false
draft: false
tags:
  - docs
  - release
description: New feature in AstroPaper v1.4.0, introducing dynamic OG image generation for blog posts.
---

AstroPaper v1.4.0 的新功能，为博客文章引入动态 OG 图片生成。

## 目录

## 简介

OG 图片（又称社交图片）在社交媒体互动中扮演着重要角色。如果你不知道 OG 图片是什么，它是当我们在社交媒体（如 Facebook、Discord 等）上分享网站 URL 时显示的图片。

> 严格来说，Twitter 使用的社交图片并不叫作 OG 图片。不过，在这篇文章中，我会用 OG 图片来统称所有类型的社交图片。

## 默认/静态 OG 图片（旧方式）

AstroPaper 已经提供了一种为博客文章添加 OG 图片的方法。作者可以在 frontmatter 的 `ogImage` 中指定 OG 图片。即使作者没有在 frontmatter 中定义 OG 图片，默认的 OG 图片也会被用作回退（即 `public/astropaper-og.jpg`）。但问题在于默认的 OG 图片是静态的，这意味着所有未在 frontmatter 中指定 OG 图片的文章都会使用相同的默认 OG 图片，尽管每篇文章的标题/内容各不相同。

## 动态 OG 图片

为每篇文章生成动态 OG 图片，可以让作者不必为每一篇博客文章都指定 OG 图片。此外，这还能避免所有文章使用相同的回退 OG 图片。

在 AstroPaper v1.4.0 中，使用了 Vercel 的 [Satori](https://github.com/vercel/satori) 包来生成动态 OG 图片。

动态 OG 图片将在构建时针对满足以下条件的博客文章生成：
- 未在 frontmatter 中指定 OG 图片
- 未标记为草稿。

## AstroPaper 动态 OG 图片的结构

AstroPaper 的动态 OG 图片包含_博客文章标题_、_作者名称_和_站点标题_。作者名称和站点标题将从 **"src/config.ts"** 文件中的 `SITE.author` 和 `SITE.title` 获取。标题从博客文章 frontmatter 的 `title` 字段生成。  
![Example Dynamic OG Image link](https://user-images.githubusercontent.com/53733092/209704501-e9c2236a-3f4d-4c67-bab3-025aebd63382.png)

### 非拉丁字符问题

标题中包含非拉丁字符时，默认无法正常显示。要解决这个问题，我们需要将 `loadGoogleFont.ts` 中的 `fontsConfig` 替换为你偏好的字体。

```ts file=src/utils/loadGoogleFont.ts
async function loadGoogleFonts(
  text: string
): Promise<
  Array<{ name: string; data: ArrayBuffer; weight: number; style: string }>
> {
  const fontsConfig = [
    {
      name: "Noto Sans JP",
      font: "Noto+Sans+JP",
      weight: 400,
      style: "normal",
    },
    {
      name: "Noto Sans JP",
      font: "Noto+Sans+JP:wght@700",
      weight: 700,
      style: "normal",
    },
    { name: "Noto Sans", font: "Noto+Sans", weight: 400, style: "normal" },
    {
      name: "Noto Sans",
      font: "Noto+Sans:wght@700",
      weight: 700,
      style: "normal",
    },
  ];
  // ...
}
```

> 更多信息请查看[此 PR](https://github.com/satnaing/astro-paper/pull/318)。

## 权衡与取舍

虽然这是一个不错的功能，但也存在取舍。每张 OG 图片大约需要一秒钟来生成。一开始可能不太明显，但随着博客文章数量的增加，你可能想要禁用此功能。由于每张 OG 图片都需要时间来生成，图片数量越多，构建时间将线性增长。

例如：如果一张 OG 图片需要一秒钟生成，那么 60 张图片大约需要一分钟，600 张图片大约需要 10 分钟。随着内容规模的扩大，这会显著影响构建时间。

相关 Issue：[#428](https://github.com/satnaing/astro-paper/issues/428)

## 局限性

在撰写本文时，[Satori](https://github.com/vercel/satori) 还比较新，尚未达到正式大版本。因此，这个动态 OG 图片功能仍有一些局限性。

- 此外，RTL（从右到左）语言还不支持。
- 在标题中[使用 emoji](https://github.com/vercel/satori#emojis) 可能会有点棘手。
