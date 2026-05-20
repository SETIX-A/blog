---
author: FjellOverflow
pubDatetime: 2024-07-25T11:11:53Z
modDatetime: 2025-03-12T12:28:53Z
title: How to integrate Giscus comments into AstroPaper
slug: how-to-integrate-giscus-comments
featured: false
draft: false
tags:
  - astro
  - blog
  - docs
description: Comment function on a static blog hosted on GitHub Pages with Giscus.
---

将轻量静态博客托管在 [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site) 这样的平台上有很多优势，但也会失去一些交互性。幸运的是，[Giscus](https://giscus.app/) 提供了在静态网站上嵌入用户评论的方法。

## 目录

## Giscus 的工作原理

[Giscus 使用 GitHub API](https://github.com/giscus/giscus?tab=readme-ov-file#how-it-works) 读取和存储 GitHub 用户在仓库关联的 `Discussions` 中发表的评论。

将 Giscus 的客户端脚本包嵌入到你的网站，配置正确的仓库 URL，用户就可以查看和发表评论（需要登录 GitHub）。

这种方法是无服务器的，因为评论存储在 GitHub 上，并在客户端动态加载，因此非常适合静态博客，比如 AstroPaper。

## 设置 Giscus

Giscus 可以轻松地在 [giscus.app](https://giscus.app/) 上设置，但我还是会简要概述一下流程。

### 前提条件

让 Giscus 正常工作的前提条件是：

- 仓库是[公开的](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility#making-a-repository-public)
- 已安装 [Giscus app](https://github.com/apps/giscus)
- 仓库已启用 [Discussions](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/enabling-or-disabling-github-discussions-for-a-repository) 功能

如果由于任何原因无法满足这些条件中的任何一条，很遗憾，Giscus 就无法集成。

### 配置 Giscus

接下来，需要配置 Giscus。在大多数情况下，预选的默认值都是合适的，只有当你出于特定原因且清楚自己在做什么时才需要修改它们。不必太担心选错，稍后你随时可以调整配置。

不过你需要：

- 为 UI 选择正确的语言
- 指定要连接的 GitHub 仓库，通常是包含你在 GitHub Pages 上托管的静态 AstroPaper 博客的仓库
- 如果你想确保没有人能直接在 GitHub 上随机创建评论，可以在 GitHub 上创建并设置一个 `Announcement` 类型的 discussion
- 定义配色方案

配置完设置后，Giscus 会为你生成一个 `<script>` 标签，你在接下来的步骤中会用到它。

## 简单的脚本标签

你现在应该有一个类似这样的 script 标签：

```html
<script
  src="https://giscus.app/client.js"
  data-repo="[ENTER REPO HERE]"
  data-repo-id="[ENTER REPO ID HERE]"
  data-category="[ENTER CATEGORY NAME HERE]"
  data-category-id="[ENTER CATEGORY ID HERE]"
  data-mapping="pathname"
  data-strict="0"
  data-reactions-enabled="1"
  data-emit-metadata="0"
  data-input-position="bottom"
  data-theme="preferred_color_scheme"
  data-lang="en"
  crossorigin="anonymous"
  async
></script>
```

只需将其添加到网站的源代码中即可。如果你正在使用 AstroPaper 并希望为文章启用评论，最有可能的做法是打开 `PostDetails.astro`，将其粘贴到你希望评论出现的合适位置，也许在「Share this post on:」按钮下方。

```astro file=src/layouts/PostDetails.astro
<Layout {...layoutProps}>
  <main>
    <ShareLinks />

    <!-- [!code ++:6] -->
    <script
      src="https://giscus.app/client.js"
      data-repo="[ENTER REPO HERE]"
      data-repo-id="[ENTER REPO ID HERE]"
      data-category="[ENTER CATEGORY NAME HERE]"
      data-category-id="[ENTER CATEGORY ID HERE]"></script>
  </main>
  <Footer />
</Layout>
```

就这样！你已经成功在 AstroPaper 中集成了评论功能！

## 支持亮色/暗色主题的 React 组件

嵌入在布局中的 script 标签相当静态，Giscus 的配置（包括 `theme`）被硬编码在布局中。考虑到 AstroPaper 具有亮色/暗色主题切换功能，如果评论能够与网站的其他部分一起在亮色和暗色主题之间无缝切换，那将会更好。为了实现这一点，需要一种更复杂的嵌入 Giscus 的方式。

首先，我们要安装 [Giscus 的 React 组件](https://www.npmjs.com/package/@giscus/react)：

```bash
npm i @giscus/react && npx astro add react
```

然后我们在 `src/components` 中创建一个新的 `Comments.tsx` React 组件：

```tsx file=src/components/Comments.tsx
import Giscus, { type Theme } from "@giscus/react";
import { GISCUS } from "@/constants";
import { useEffect, useState } from "react";

interface CommentsProps {
  lightTheme?: Theme;
  darkTheme?: Theme;
}

export default function Comments({
  lightTheme = "light",
  darkTheme = "dark",
}: CommentsProps) {
  const [theme, setTheme] = useState(() => {
    const currentTheme = localStorage.getItem("theme");
    const browserTheme = window.matchMedia("(prefers-color-scheme: dark)")
      .matches
      ? "dark"
      : "light";

    return currentTheme || browserTheme;
  });

  useEffect(() => {
    const mediaQuery = window.matchMedia("(prefers-color-scheme: dark)");
    const handleChange = ({ matches }: MediaQueryListEvent) => {
      setTheme(matches ? "dark" : "light");
    };

    mediaQuery.addEventListener("change", handleChange);

    return () => mediaQuery.removeEventListener("change", handleChange);
  }, []);

  useEffect(() => {
    const themeButton = document.querySelector("#theme-btn");
    const handleClick = () => {
      setTheme(prevTheme => (prevTheme === "dark" ? "light" : "dark"));
    };

    themeButton?.addEventListener("click", handleClick);

    return () => themeButton?.removeEventListener("click", handleClick);
  }, []);

  return (
    <div className="mt-8">
      <Giscus theme={theme === "light" ? lightTheme : darkTheme} {...GISCUS} />
    </div>
  );
}
```

这个 React 组件不仅封装了原生的 Giscus 组件，还引入了额外的 props，即 `lightTheme` 和 `darkTheme`。利用两个事件监听器，Giscus 评论将与网站主题保持一致，在网站或浏览器主题变更时动态地在暗色和亮色主题之间切换。

我们还需要定义 `GISCUS` 配置，最佳位置是在 `constants.ts` 中：

```ts file=src/constants.ts
import type { GiscusProps } from "@giscus/react";

...

export const GISCUS: GiscusProps = {
  repo: "[ENTER REPO HERE]",
  repoId: "[ENTER REPO ID HERE]",
  category: "[ENTER CATEGORY NAME HERE]",
  categoryId: "[ENTER CATEGORY ID HERE]",
  mapping: "pathname",
  reactionsEnabled: "0",
  emitMetadata: "0",
  inputPosition: "bottom",
  lang: "en",
  loading: "lazy",
};
```

请注意，在此处指定 `theme` 将覆盖 `lightTheme` 和 `darkTheme` props，从而导致静态主题设置，类似于之前使用 `<script>` 标签嵌入 Giscus 的方式。

要完成整个过程，将新的 Comments 组件添加到 `PostDetails.astro` 中（替换前一步骤中的 `script` 标签）。

```jsx file=src/layouts/PostDetails.astro
// [!code ++:1]
import Comments from "@/components/Comments";

<ShareLinks />

// [!code ++:1]
<Comments client:only="react" />

<hr class="my-6 border-dashed" />

<Footer />
```

就这样，完成了！
