---
pubDatetime: 2025-03-08T08:18:19.693Z
title: AstroPaper 5.0
slug: astro-paper-v5
featured: true
ogImage: ../../../assets/images/AstroPaper-v5.png
tags:
  - release
description: "AstroPaper v5: keep the clean look, updates under the hood."
---

终于，期待已久的 AstroPaper v5 正式发布了。AstroPaper v5 保持了同样简洁清爽的外观，但底层带来了重大更新。

![AstroPaper v5](@/assets/images/AstroPaper-v5.png)

## 目录

## 重大变更

### 升级到 Astro v5 [#455](https://github.com/satnaing/astro-paper/pull/455)

AstroPaper 现在搭载 Astro v5，带来了随之而来的所有新功能和改进。

### Tailwind v4

AstroPaper 已升级到 Tailwind v4，底层包含了许多样式变更。`tailwind.config.js` 文件已被移除，现在所有配置都位于 `src/styles/global.css` 文件中。排版相关样式已被提取并移至 `src/styles/typography.css`。

由于 TailwindCSS v4 的新行为，组件内 `<style>` 块中的样式已被移除，替换为内联 Tailwind 类。

此外，UI 中的颜色调色板已更新。新的调色板现在仅包含五种颜色：

```css
:root,
html[data-theme="light"] {
  --background: #fdfdfd;
  --foreground: #282728;
  --accent: #006cac;
  --muted: #e6e6e6;
  --border: #ece9e9;
}

html[data-theme="dark"] {
  --background: #212737;
  --foreground: #eaedf3;
  --accent: #ff6b01;
  --muted: #343f60bf;
  --border: #ab4b08;
}
```

### 移除 React + Fuse.js，改用 Pagefind 搜索

在之前的版本中，React.js 和 Fuse.js 被用于搜索功能和 OG 图片生成。在 AstroPaper v5 中，React.js 已被移除，替换为 [Pagefind](https://pagefind.app/)，一个静态站点搜索工具。

搜索体验与之前版本几乎相同，但得益于 Pagefind，现在所有内容（不仅仅是标题和描述）都会被索引并可搜索。

在开发模式中使用 Pagefind 的想法受[这篇博客文章](https://chrispennington.blog/blog/pagefind-static-search-for-astro-sites/)启发。

### 更新导入别名

导入别名已从 `@directory` 更新为 `@/directory`，这意味着你现在需要像这样导入：

```astro
---
import { slugifyStr } from "@/utils/slugify";
import IconHash from "@/assets/icons/IconHash.svg";
---
```

### 迁移至 `pnpm`

AstroPaper 已从 `npm` 切换到 `pnpm`，提供更快、更高效的包管理。

### 将图标/SVG 替换为 Astro 的 SVG 组件

AstroPaper v5 将内联 SVG 替换为 Astro 的实验性 [SVG 组件](https://docs.astro.build/en/reference/experimental-flags/svg/)。此更新减少了 `socialIcons` 对象中对预定义 SVG 代码的需求，使代码库更简洁、更易于维护。

### 分离常量与配置

项目结构已重新组织。`src/config.ts` 文件现在仅包含 `SITE` 对象，其中存放项目的主要配置。所有常量，如 `LOCALE`、`SOCIALS` 和 `SHARE_LINKS`，已移至 `src/constants.ts` 文件。

## 其他值得注意的变更

- 博客文章目录已从 `src/content/blog/` 更新为 `src/data/blog/`。
- 集合定义文件（`src/content/config.ts`）现已替换为 `src/content.config.ts`。
- 各种依赖项已升级以提升性能和安全性。
- 移除了 `IBM Plex Mono` 字体，切换为默认的系统等宽字体。
- 「返回」按钮的逻辑已更新。现在，AstroPaper v5 不再触发浏览器的历史 API，而是使用浏览器会话临时存储返回 URL。如果会话中没有返回 URL，则会重定向到首页。
- 还有一些微小的样式和布局变更。

## 结语

AstroPaper v5 带来了许多变化，但核心体验保持不变。在保持 AstroPaper 一贯的简洁和极简设计的同时，享受更流畅、更高效的博客平台！

欢迎探索这些变化并分享你的想法。一如既往，感谢你的支持！

如果你喜欢这个主题，请考虑为仓库点亮 Star。你也可以通过 GitHub Sponsors 支持我，或者如果你想的话，可以请我喝杯咖啡。当然，这些行为完全是可选的，并非必须。

享受吧！

[Sat Naing](https://satnaing.dev/)
