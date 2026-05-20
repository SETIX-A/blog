---
author: Sat Naing
pubDatetime: 2022-09-25T15:20:35Z
modDatetime: 2026-01-09T15:00:15.170Z
title: Customizing AstroPaper theme color schemes
featured: false
draft: false
tags:
  - color-schemes
  - docs
description:
  How you can enable/disable light & dark mode; and customize color schemes
  of AstroPaper theme.
---

本文将介绍如何为网站启用/禁用亮色与暗色模式。此外，你还将学习如何自定义整个网站的配色方案。

## 目录

## 启用/禁用亮色与暗色模式

AstroPaper 主题默认包含亮色和暗色模式。换句话说，会有两套配色方案——一套用于亮色模式，另一套用于暗色模式。可以通过 `SITE` 配置对象禁用此默认行为。

```js file="src/config.ts"
export const SITE = {
  website: "https://astro-paper.pages.dev/", // replace this with your deployed domain
  author: "Sat Naing",
  profile: "https://satnaing.dev/",
  desc: "A minimal, responsive and SEO-friendly Astro blog theme.",
  title: "AstroPaper",
  ogImage: "astropaper-og.jpg",
  lightAndDarkMode: true, // [!code highlight]
  postPerIndex: 4,
  postPerPage: 4,
  scheduledPostMargin: 15 * 60 * 1000, // 15 minutes
  showArchives: true,
  showBackButton: true, // show back button in post detail
  editPost: {
    enabled: true,
    text: "Suggest Changes",
    url: "https://github.com/satnaing/astro-paper/edit/main/",
  },
  dynamicOgImage: true,
  lang: "en", // html lang code. Set this empty and default will be "en"
  timezone: "Asia/Bangkok", // Default global timezone (IANA format) https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
} as const;
```

要禁用亮色与暗色模式，请将 `SITE.lightAndDarkMode` 设置为 `false`。

## 选择初始配色方案

默认情况下，如果禁用了 `SITE.lightAndDarkMode`，我们将只会获得系统的首选配色方案。

因此，要选择一个初始配色方案而不是系统首选配色方案，我们需要在 `theme.ts` 中的 `initialColorScheme` 变量里设置配色方案。

```ts file="src/scripts/theme.ts"
// Initial color scheme
// Can be "light", "dark", or empty string for system's prefers-color-scheme
const initialColorScheme = ""; // "light" | "dark" // [!code hl]

function getPreferTheme(): string {
  // get theme data from local storage (user's explicit choice)
  const currentTheme = localStorage.getItem("theme");
  if (currentTheme) return currentTheme;

  // return initial color scheme if it is set (site default)
  if (initialColorScheme) return initialColorScheme;

  // return user device's prefer color scheme (system fallback)
  return window.matchMedia("(prefers-color-scheme: dark)").matches
    ? "dark"
    : "light";
}

// ...
```

**initialColorScheme** 变量可以持有两个值：`"light"`、`"dark"`。如果你不想指定初始配色方案，可以保留空字符串（默认值）。

- `""` - 系统的首选配色方案。（默认值）
- `"light"` - 使用亮色模式作为初始配色方案。
- `"dark"` - 使用暗色模式作为初始配色方案。

<details>
<summary>为什么 initialColorScheme 不在 config.ts 中？</summary>
为了避免页面重新加载时颜色闪烁，我们必须在页面加载时尽早放置主题初始化的 JavaScript 代码。主题脚本分为两部分：在 `<head>` 中的最小化内联脚本（立即设置主题），以及异步加载的完整脚本。这种方法可以在保持最佳性能的同时防止 FOUC（未样式化内容闪烁）。
</details>

## 自定义配色方案

AstroPaper 主题的亮色和暗色配色方案都可以在 `global.css` 文件中自定义。

```css file="src/styles/global.css"
@import "tailwindcss";
@import "./typography.css";

@custom-variant dark (&:where([data-theme=dark], [data-theme=dark] *));

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
/* ... */
```

在 AstroPaper 主题中，`:root` 和 `html[data-theme="light"]` 选择器定义了亮色配色方案，而 `html[data-theme="dark"]` 定义了暗色配色方案。

要自定义你自己的配色方案，在 `:root, html[data-theme="light"]` 中指定亮色颜色，在 `html[data-theme="dark"]` 中指定暗色颜色。

以下是颜色属性的详细说明。

| 颜色属性 | 定义与用途 |
| -------------- | ------------------------------------------------------------- |
| `--background` | 网站的主色。通常是主要背景色。 |
| `--foreground` | 网站的次要颜色。通常是文本颜色。 |
| `--accent` | 网站的重点色。链接颜色、悬停颜色等。 |
| `--muted` | 卡片和滚动条的背景色，用于悬停状态等。 |
| `--border` | 边框颜色。用于边框工具和视觉分隔。 |

以下是更改亮色配色方案的示例。

```css file="src/styles/global.css"
/* ... */
:root,
html[data-theme="light"] {
  --background: #f6eee1;
  --foreground: #012c56;
  --accent: #e14a39;
  --muted: #efd8b0;
  --border: #dc9891;
}
/* ... */
```

> 查看 AstroPaper 已为你精心制作的一些[预定义配色方案](https://astro-paper.pages.dev/posts/predefined-color-schemes/)。
