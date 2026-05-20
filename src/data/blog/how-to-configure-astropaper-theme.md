---
author: Sat Naing
pubDatetime: 2022-09-23T04:58:53Z
modDatetime: 2026-01-10T13:04:53.851Z
title: How to configure AstroPaper theme
slug: how-to-configure-astropaper-theme
featured: true
draft: false
tags:
  - configuration
  - docs
description: How you can make AstroPaper theme absolutely yours.
---

AstroPaper 是一个高度可定制的 Astro 博客主题。使用 AstroPaper，你可以根据个人喜好自定义一切。本文将介绍如何轻松地在配置文件中进行一些自定义设置。

## 目录

## 配置 SITE

重要的配置位于 `src/config.ts` 文件中。在该文件中，你会看到 `SITE` 对象，你可以在其中指定网站的主要配置。

在开发期间，将 `SITE.website` 留空是可以的。但在生产模式下，你应该在 `SITE.website` 选项中指定已部署的 URL，因为这将用于规范 URL、社交卡片 URL 等，这些对 SEO 很重要。

```js file=src/config.ts
export const SITE = {
  website: "https://astro-paper.pages.dev/", // replace this with your deployed domain
  author: "Sat Naing",
  profile: "https://satnaing.dev/",
  desc: "A minimal, responsive and SEO-friendly Astro blog theme.",
  title: "AstroPaper",
  ogImage: "astropaper-og.jpg",
  lightAndDarkMode: true,
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
  dynamicOgImage: true, // enable automatic dynamic og-image generation
  dir: "ltr", // "rtl" | "auto"
  lang: "en", // html lang code. Set this empty and default will be "en"
  timezone: "Asia/Bangkok", // Default global timezone (IANA format) https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
} as const;
```

以下是 SITE 配置选项

| 选项 | 描述 |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `website` | 你已部署的网站 URL |
| `author` | 你的名字 |
| `profile` | 你的个人/作品集网站 URL，用于更好的 SEO。如果没有，可设置为 `null` 或空字符串 `""`。 |
| `desc` | 你的网站描述。对 SEO 和社交媒体分享很有用。 |
| `title` | 你的网站名称 |
| `ogImage` | 网站的默认 OG 图片。对社交媒体分享很有用。OG 图片可以是外部图片 URL，也可以放在 `/public` 目录下。 |
| `lightAndDarkMode` | 启用或禁用网站的亮色与暗色模式。如果禁用，将使用主配色方案。此选项默认启用。 |
| `postPerIndex` | 首页「Recent」板块下显示的博客文章数量。 |
| `postPerPage` | 你可以指定每页显示多少篇文章。（例如：如果将 `SITE.postPerPage` 设置为 3，每页将只显示 3 篇文章） |
| `scheduledPostMargin` | 在生产模式下，`pubDatetime` 为未来的文章将不可见。但如果文章的 `pubDatetime` 在接下来的 15 分钟内，它将是可见的。如果你不喜欢默认的 15 分钟容差，可以设置 `scheduledPostMargin`。 |
| `showArchives` | 决定是否显示「Archives」菜单（位于「About」和「Search」菜单之间）及其对应页面。此选项默认设置为 `true`。 |
| `showBackButton` | 决定是否在每篇博客文章中显示「返回」按钮。 |
| `editPost` | 此选项允许用户通过博客文章标题下方提供编辑链接来建议修改。可以通过将 `SITE.editPost.enabled` 设置为 `false` 来禁用此功能。 |
| `dynamicOgImage` | 此选项控制当博客文章 frontmatter 中未指定 `ogImage` 时，是否[生成动态 OG 图片](https://astro-paper.pages.dev/posts/dynamic-og-image-generation-in-astropaper-blog-posts/)。如果你有大量博客文章，可能需要禁用此功能。更多详情请参阅[权衡与取舍](https://astro-paper.pages.dev/posts/dynamic-og-image-generation-in-astropaper-blog-posts/#trade-off)。 |
| `dir` | 指定整个博客的文字方向。用作 `<html dir="ltr">` 中的 [HTML dir 属性](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/dir)。支持的值：`ltr` \| `rtl` \| `auto` |
| `lang` | 用作 `<html lang="en">` 中的 HTML ISO 语言代码。默认为 `en`。 |
| `timezone` | 此选项允许你使用 [IANA 格式](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 指定时区。设置此项可确保本地和生产站点之间时间戳一致，消除时差。 |

## 更新布局宽度

整个博客的默认 `max-width` 为 `768px`（`max-w-3xl`）。如果你想更改它，可以轻松地在 `global.css` 中更新 `max-w-app` 工具类。例如：

```css file=src/styles/global.css
@utility max-w-app {
  /* [!code --:1] */
  @apply max-w-3xl;
  /* [!code ++:1] */
  @apply max-w-4xl xl:max-w-5xl;
}
```

你可以在 [Tailwind CSS 文档](https://tailwindcss.com/docs/max-width) 中探索更多 `max-width` 值。

## 配置 Logo 或标题

在 AstroPaper v5 之前，你可以在 `src/config.ts` 文件中的 `LOGO_IMAGE` 对象里更新网站名称/Logo。然而，在 AstroPaper v5 中，此选项已被移除，改为使用 Astro 内置的 SVG 和 Image 组件。

![An arrow pointing at the website logo](https://res.cloudinary.com/noezectz/v1663911318/astro-paper/AstroPaper-logo-config_goff5l.png)

你可以选择以下 3 种方案：

### 方案一：SITE 标题文本

这是最简单的方案。你只需在 `src/config.ts` 文件中更新 `SITE.title` 即可。

### 方案二：Astro 的 SVG 组件

如果你想使用 SVG Logo，可以选择此方案。

- 首先将 SVG 添加到 `src/assets` 目录中。（例如：`src/assets/dummy-logo.svg`）
- 然后在 `Header.astro` 中导入该 SVG

  ```astro file=src/components/Header.astro
  ---
  // ...
  import DummyLogo from "@/assets/dummy-logo.svg";
  ---
  ```

- 最后，用导入的 Logo 替换 `{SITE.title}`。

  ```html
  <a
    href="/"
    class="absolute py-1 text-left text-2xl leading-7 font-semibold whitespace-nowrap sm:static"
  >
    <DummyLogo class="scale-75 dark:invert" />
    <!-- {SITE.title} -->
  </a>
  ```

这种方案的最大好处是你可以根据需要自定义 SVG 样式。在上面的示例中，你可以看到 SVG Logo 颜色如何在暗色模式下被反转。

### 方案三：Astro 的 Image 组件

如果你的 Logo 是图片而非 SVG，可以使用 Astro 的 Image 组件。

- 将你的 Logo 添加到 `src/assets` 目录中。（例如：`src/assets/dummy-logo.png`）
- 在 `Header.astro` 中导入 `Image` 和你的 Logo

  ```astro file=src/components/Header.astro
  ---
  // ...
  import { Image } from "astro:assets";
  import dummyLogo from "@/assets/dummy-logo.png";
  ---
  ```

- 然后，用导入的 Logo 替换 `{SITE.title}`。

  <!-- prettier-ignore -->
  ```html
  <a
    href="/"
    class="absolute py-1 text-left text-2xl leading-7 font-semibold whitespace-nowrap sm:static"
  >
    <Image src="{dummyLogo}" alt="Dummy Blog" class="dark:invert" />
    <!-- {SITE.title} -->
  </a>
  ```

通过此方案，你仍然可以使用 CSS 类来调整图片的外观。但这可能并不总能满足你的需求。如果你需要根据亮色或暗色模式显示不同的 Logo 图片，请查看 `Header.astro` 组件中如何处理亮色/暗色图标。

## 配置社交链接

![An arrow pointing at social link icons](https://github.com/user-attachments/assets/8b895400-d088-442f-881b-02d2443e00cf)

你可以在 `constants.ts` 中的 `SOCIALS` 对象里配置社交链接。

```ts file=src/constants.ts
export const SOCIALS = [
  {
    name: "GitHub",
    href: "https://github.com/satnaing/astro-paper",
    linkTitle: ` ${SITE.title} on GitHub`,
    icon: IconGitHub,
  },
  {
    name: "X",
    href: "https://x.com/username",
    linkTitle: `${SITE.title} on X`,
    icon: IconBrandX,
  },
  {
    name: "LinkedIn",
    href: "https://www.linkedin.com/in/username/",
    linkTitle: `${SITE.title} on LinkedIn`,
    icon: IconLinkedin,
  },
  {
    name: "Mail",
    href: "mailto:yourmail@gmail.com",
    linkTitle: `Send an email to ${SITE.title}`,
    icon: IconMail,
  },
] as const;
```

## 配置分享链接

你可以在 `src/constants.ts` 中的 `SHARE_LINKS` 对象里配置分享链接。

![An arrow pointing at share link icons](https://github.com/user-attachments/assets/4f930b68-b625-45df-8c41-e076dd2b838e)

## 配置字体

AstroPaper 使用 Astro 的[实验性字体 API](https://docs.astro.build/en/reference/experimental-flags/fonts/)，默认字体为 [Google Sans Code](https://fonts.google.com/specimen/Google+Sans+Code)。这提供了跨平台一致的排版体验，并包含自动字体优化，包括预加载和缓存。

### 使用默认字体

字体在 `astro.config.ts` 中自动配置，并在 `Layout.astro` 中加载。使用默认的 Google Sans Code 字体无需额外配置。

### 自定义字体

要使用不同的字体，你需要在三个地方进行更新：

1. **更新 `astro.config.ts` 中的字体配置：**

```ts file=astro.config.ts
import { defineConfig, fontProviders } from "astro/config";

export default defineConfig({
  // ...
  experimental: {
    fonts: [
      {
        name: "Your Font Name", // [!code highlight]
        cssVariable: "--font-your-font", // [!code highlight]
        provider: fontProviders.google(),
        fallbacks: ["monospace"],
        weights: [300, 400, 500, 600, 700],
        styles: ["normal", "italic"],
      },
    ],
  },
});
```

1. **更新 `Layout.astro` 中的 Font 组件：**

```astro file=src/layouts/Layout.astro
---
import { Font } from "astro:assets";
// ...
---

<head>
  <!-- ... -->
  // [!code highlight:4]
  <Font
    cssVariable="--font-your-font"
    preload={[{ subset: "latin", weight: 400, style: "normal" }]}
  />
  <!-- ... -->
</head>
```

1. **更新 `global.css` 中的 CSS 变量映射：**

```css file=src/styles/global.css
@theme inline {
  --font-app: var(--font-your-font); /* [!code highlight] */
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-accent: var(--accent);
  --color-muted: var(--muted);
  --color-border: var(--border);
}
```

`--font-app` 变量通过 `font-app` Tailwind 工具类在整个主题中使用，因此只需更新这一个变量，即可将自定义字体应用到所有地方。

> **注意**：确保字体名称与 [Google Fonts](https://fonts.google.com) 上显示的名称完全一致。对于其他字体提供商或本地字体，请参阅 [Astro 实验性字体 API 文档](https://docs.astro.build/en/reference/experimental-flags/fonts/)。

## 结语

以上就是关于如何自定义此主题的简要说明。如果你会一些编程，还可以进行更多自定义。有关自定义样式，请阅读[这篇文章](https://astro-paper.pages.dev/posts/customizing-astropaper-theme-color-schemes/)。感谢阅读。✌🏻
