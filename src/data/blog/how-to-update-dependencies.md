---
title: How to update dependencies of AstroPaper
author: Sat Naing
pubDatetime: 2023-07-20T15:33:05.569Z
slug: how-to-update-dependencies
featured: false
draft: false
ogImage: ../../assets/images/forrest-gump-quote.png
tags:
  - FAQ
description: How to update project dependencies and AstroPaper template.
---

更新项目依赖项可能是一件繁琐的事情。然而，忽视更新项目依赖项也不是一个好主意 😬。在这篇文章中，我将分享我通常如何更新项目，以 AstroPaper 为例。不过，这些步骤也适用于其他 JS/Node 项目。

![Forrest Gump Fake Quote](@/assets/images/forrest-gump-quote.png)

## 目录

## 更新包依赖项

有几种更新依赖项的方法，我尝试过各种方法以找到最简单的路径。一种方法是使用 `npm install package-name@latest` 手动更新每个包。这种方法是最直接的更新方式。然而，它可能不是最高效的选择。

我推荐的更新依赖项的方式是使用 [npm-check-updates 包](https://www.npmjs.com/package/npm-check-updates)。freeCodeCamp 上有一篇很好的[文章](https://www.freecodecamp.org/news/how-to-update-npm-dependencies/)介绍了它，所以我不会详细解释它是什么以及如何使用它。相反，我将展示我的典型做法。

首先，全局安装 `npm-check-updates` 包。

```bash
npm install -g npm-check-updates
```

在进行任何更新之前，最好先检查所有可以更新的新依赖项。

```bash
ncu
```

大多数情况下，补丁（patch）级别的依赖项可以在完全不影响项目的情况下更新。因此，我通常通过运行 `ncu -i --target patch` 或 `ncu -u --target patch` 来更新补丁依赖项。区别在于 `ncu -u --target patch` 会更新所有补丁，而 `ncu -i --target patch` 会给出选项以切换要更新哪个包。具体采用哪种方式取决于你。

下一步涉及更新次版本（minor）依赖项。次版本包的更新通常不会破坏项目，但查看相应包的发布说明总是好的。这些次版本更新通常包含一些可以应用到我们项目中的很酷的功能。

```bash
ncu -i --target minor
```

最后但同样重要的是，依赖项中可能有一些主版本（major）的包更新。因此，通过运行以下命令检查剩余的依赖项更新：

```bash
ncu -i
```

如果有任何主版本更新（或一些你仍需要进行的更新），上述命令将输出这些剩余的包。如果包是主版本更新，你必须非常小心，因为这很可能会破坏整个项目。因此，请仔细阅读相应的发布说明（或）文档，并做出相应的更改。

如果你运行 `ncu -i` 后发现没有更多需要更新的包，_**恭喜！！！**_ 你已经成功更新了项目中的所有依赖项。

## 更新 AstroPaper 模板

像其他开源项目一样，AstroPaper 也在不断演进，进行 bug 修复、功能更新等。因此，如果你是将 AstroPaper 作为模板使用的人，当有新版本发布时，你可能也想要更新模板。

问题是，你可能已经根据自己的喜好对模板进行了修改。因此，我无法精确地展示 **"一刀切的完美方法"** 来将模板更新到最新版本。不过，这里有一些提示可以帮助你在不破坏你仓库的情况下更新模板。请记住，大多数情况下，仅更新包依赖项可能对你来说已经足够了。

### 需要留意的文件和目录

在大多数情况下，你可能不想覆盖的文件和目录（因为你很可能已经修改过这些文件）是 `src/content/blog/`、`src/config.ts`、`src/pages/about.md`，以及其他资源和样式文件，如 `public/` 和 `src/styles/base.css`。

如果你只对模板做了最低限度的修改，那么除了上述文件和目录之外，将其他所有内容替换为最新的 AstroPaper 应该没问题。这就像纯粹的 Android OS 和其他厂商特定的 OS（如 OneUI）一样。你对基础修改得越少，需要更新的就越少。

你可以手动逐个替换每个文件，也可以使用 Git 的魔法来更新一切。我不会展示手动替换的过程，因为那非常直接。如果你对那种直接但低效的方法不感兴趣，请继续耐心看下去 🐻。

### 使用 Git 更新 AstroPaper

**重要提示！！！**

> 只有在你懂得如何解决合并冲突的情况下才执行以下操作。否则，你最好手动替换文件或仅更新依赖项。

首先，在你的项目中添加 astro-paper 作为远程仓库。

```bash
git remote add astro-paper https://github.com/satnaing/astro-paper.git
```

切换到一个新分支以便更新模板。如果你知道自己在做什么并且对自己的 Git 技能有信心，可以跳过此步骤。

```bash
git checkout -b build/update-astro-paper
```

然后，通过运行以下命令从 astro-paper 拉取更改：

```bash
git pull astro-paper main
```

如果你遇到 `fatal: refusing to merge unrelated histories` 错误，可以通过运行以下命令来解决：

```bash
git pull astro-paper main --allow-unrelated-histories
```

运行上述命令后，你可能会在项目中遇到冲突。你需要手动解决这些冲突，并根据自己的需求做出必要的调整。

解决冲突后，请全面测试你的博客，确保一切按预期工作。检查你的文章、组件以及你做的任何自定义修改。

一旦你对结果满意，就可以将更新分支合并到主分支中（仅当你是在另一个分支中更新模板时）。恭喜！你已经成功将模板更新到最新版本。你的博客现在是最新的，准备大放异彩！🎉

## 结语

在这篇文章中，我分享了一些关于更新依赖项和 AstroPaper 模板的见解和流程。我真诚地希望这篇文章对你有所帮助，并助你更高效地管理项目。

如果你有任何替代或改进的更新依赖项/AstroPaper 的方法，我很乐意听取你的意见。因此，请随时在仓库中发起讨论、给我发邮件或提交 Issue。你的意见和想法将受到高度赞赏！

请理解我最近日程安排比较忙，可能无法快速回复。但我保证会尽快回复你。😬

感谢你花时间阅读这篇文章，祝你项目一切顺利！
