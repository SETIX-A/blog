---
author: Simon Smale
pubDatetime: 2024-01-03T20:40:08Z
modDatetime: 2024-01-08T18:59:05Z
title: How to use Git Hooks to set Created and Modified Dates
featured: false
draft: false
tags:
  - docs
  - FAQ
canonicalURL: https://smale.codes/posts/setting-dates-via-git-hooks/
description: How to use Git Hooks to set your Created and Modified Dates on AstroPaper
---

在这篇文章中，我将介绍如何使用 pre-commit Git 钩子来自动化设置 AstroPaper 博客主题 frontmatter 中的创建日期（`pubDatetime`）和修改日期（`modDatetime`）。

## 目录

## 让它们无处不在

[Git 钩子](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) 非常适合自动化任务，例如将分支名称[添加](https://gist.github.com/SSmale/3b380e5bbed3233159fb7031451726ea)到提交信息中或[检查](https://itnext.io/using-git-hooks-to-enforce-branch-naming-policy-ffd81fa01e5e)分支名称，或者[阻止你提交明文密钥](https://gist.github.com/SSmale/367deee757a9b2e119d241e120249000)。它们最大的缺点在于客户端钩子是每台机器独立的。

你可以通过创建一个 `hooks` 目录并手动将它们复制到 `.git/hooks` 目录或设置符号链接来解决这个问题，但这都需要你记得去设置，而这不是我擅长的事情。

由于这个项目使用 npm，我们可以利用一个名为 [Husky](https://typicode.github.io/husky/) 的包（AstroPaper 中已经安装）来自动为我们安装钩子。

> 更新！在 AstroPaper [v4.3.0](https://github.com/satnaing/astro-paper/releases/tag/v4.3.0) 中，pre-commit 钩子已被移除，改为使用 GitHub Actions。不过，你可以轻松地自行[安装 Husky](https://typicode.github.io/husky/get-started.html)。

## 钩子

由于我们希望在提交代码时运行此钩子来更新日期，并使其成为我们变更的一部分，我们将使用 `pre-commit` 钩子。这个钩子已经在此 AstroPaper 项目中设置好了，但如果还没有设置，你需要运行 `npx husky add .husky/pre-commit 'echo "This is our new pre-commit hook"'`。

导航到 `hooks/pre-commit` 文件，我们将添加以下一个或两个代码段。

### 当文件被编辑时更新修改日期

---

更新：

本节已更新为新版本的钩子，更加智能。现在它不会在文章发布之前递增 `modDatetime`。在首次发布时，将草稿状态设置为 `first`，然后见证魔法发生。

---

```shell
# Modified files, update the modDatetime
git diff --cached --name-status |
grep -i '^M.*\.md$' |
while read _ file; do
  filecontent=$(cat "$file")
  frontmatter=$(echo "$filecontent" | awk -v RS='---' 'NR==2{print}')
  draft=$(echo "$frontmatter" | awk '/^draft: /{print $2}')
  if [ "$draft" = "false" ]; then
    echo "$file modDateTime updated"
    cat $file | sed "/---.*/,/---.*/s/^modDatetime:.*$/modDatetime: $(date -u "+%Y-%m-%dT%H:%M:%SZ")/" > tmp
    mv tmp $file
    git add $file
  fi
  if [ "$draft" = "first" ]; then
    echo "First release of $file, draft set to false and modDateTime removed"
    cat $file | sed "/---.*/,/---.*/s/^modDatetime:.*$/modDatetime:/" | sed "/---.*/,/---.*/s/^draft:.*$/draft: false/" > tmp
    mv tmp $file
    git add $file
  fi
done
```

`git diff --cached --name-status` 从 Git 中获取已暂存待提交的文件。输出类似于：

```shell
A       src/content/blog/setting-dates-via-git-hooks.md
```

开头的字母表示执行了何种操作，在上面的示例中，文件已被添加（Added）。修改过的文件显示为 `M`。

我们将该输出通过管道传入 grep 命令，在其中逐行查找已被修改的文件。该行需要以 `M` 开头（`^(M)`），之后有任意数量的字符（`.*`），并以 `.md` 文件扩展名结尾（`.(md)$`）。这将过滤掉不是修改过的 markdown 文件的行 `egrep -i "^(M).*\.(md)$"`。

---

#### 改进 —— 更精确

可以改为仅在 `blog` 目录中查找 markdown 文件，因为只有这些文件才具有正确的 frontmatter。

---

正则表达式将捕获两部分：字母和文件路径。我们将把这个列表通过管道传入 while 循环，迭代匹配的行，并将字母赋值给 `a`，路径赋值给 `b`。目前我们暂时忽略 `a`。

要知道文件的草稿状态，我们需要它的 frontmatter。在以下代码中，我们使用 `cat` 获取文件内容，然后使用 `awk` 按 frontmatter 分隔符（`---`）分割文件，取第二块（即 frontmatter，位于 `---` 之间的部分）。然后再次使用 `awk` 查找 draft 键并打印其值。

```shell
  filecontent=$(cat "$file")
  frontmatter=$(echo "$filecontent" | awk -v RS='---' 'NR==2{print}')
  draft=$(echo "$frontmatter" | awk '/^draft: /{print $2}')
```

现在我们有了 `draft` 的值，将执行以下三种操作之一：将 modDatetime 设置为当前时间（当 draft 为 false 时 `if [ "$draft" = "false" ]; then`），清空 modDatetime 并将 draft 设置为 false（当 draft 设置为 first 时 `if [ "$draft" = "first" ]; then`），或者什么也不做（其他情况）。

接下来的 sed 命令部分对我来说有点神奇，因为我不常使用它，它是从[另一篇关于类似操作的博客文章](https://mademistakes.com/notes/adding-last-modified-timestamps-with-git/)中复制的。本质上，它是在文件的 frontmatter 标记（`---`）内部查找 `pubDatetime:` 键，获取完整行，并将其替换为 `pubDatetime: $(date -u "+%Y-%m-%dT%H:%M:%SZ")/"` —— 相同的键加上格式正确的当前日期时间。

此替换是在整个文件的上下文中进行的，因此我们将其放入一个临时文件（`> tmp`），然后将新文件移动（`mv`）到旧文件的位置，覆盖它。然后将其添加到 Git 中，就像我们自己做了这个更改一样。

---

#### 注意

要使 `sed` 正常工作，frontmatter 中需要已经有 `modDatetime` 键。为了让应用能够以空白日期构建，你还需要做一些其他更改，请参阅[下文](#empty-moddatetime-changes)。

---

### 为新文件添加日期

为新文件添加日期的过程与上述相同，但这次我们查找的是已添加（`A`）的行，并且将替换 `pubDatetime` 值。

```shell
# New files, add/update the pubDatetime
git diff --cached --name-status | egrep -i "^(A).*\.(md)$" | while read a b; do
  cat $b | sed "/---.*/,/---.*/s/^pubDatetime:.*$/pubDatetime: $(date -u "+%Y-%m-%dT%H:%M:%SZ")/" > tmp
  mv tmp $b
  git add $b
done
```

---

#### 改进 —— 只循环一次

我们可以使用 `a` 变量在循环内部进行切换，在一次循环中要么更新 `modDatetime`，要么添加 `pubDatetime`。

---

## 填充 frontmatter

如果你的 IDE 支持代码片段，可以选择创建一个自定义片段来填充 frontmatter。[AstroPaper v4 将默认附带一个 VSCode 代码片段。](https://github.com/satnaing/astro-paper/pull/206)

<video autoplay muted="muted" controls plays-inline="true" class="border border-skin-line">
  <source src="https://github.com/satnaing/astro-paper/assets/17761689/e13babbc-2d78-405d-8758-ca31915e41b0" type="video/mp4">
</video>

## 空白 `modDatetime` 的更改

为了让 Astro 编译 markdown 并完成其工作，它需要知道 frontmatter 中预期有什么。它通过 `src/content/config.ts` 中的配置来实现这一点。

为了允许键存在但值为空，我们需要编辑第 10 行，添加 `.nullable()` 函数。

```ts
const blog = defineCollection({
  type: "content",
  schema: ({ image }) =>
    z.object({
      author: z.string().default(SITE.author),
      pubDatetime: z.date(),
      modDatetime: z.date().optional(), // [!code --]
      modDatetime: z.date().optional().nullable(), // [!code ++]
      title: z.string(),
      featured: z.boolean().optional(),
      draft: z.boolean().optional(),
      tags: z.array(z.string()).default(["others"]),
      ogImage: image().or(z.string()).optional(),
      description: z.string(),
      canonicalURL: z.string().optional(),
      readingTime: z.string().optional(),
    }),
});
```

为了不让 IDE 在博客引擎文件中报错，我还做了以下操作：

1. 在 `src/layouts/Layout.astro` 的第 15 行添加了 `| null`，使其如下所示：

   ```typescript
   export interface Props {
     title?: string;
     author?: string;
     description?: string;
     ogImage?: string;
     canonicalURL?: string;
     pubDatetime?: Date;
     modDatetime?: Date | null;
   }
   ```

2. 在 `src/components/Datetime.tsx` 的第 5 行添加了 `| null`，使其如下所示：

   ```typescript
   interface DatetimesProps {
     pubDatetime: string | Date;
     modDatetime: string | Date | undefined | null;
   }
   ```
