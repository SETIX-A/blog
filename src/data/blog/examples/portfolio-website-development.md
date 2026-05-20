---
title: How Do I Develop My Portfolio Website & Blog
author: Sat Naing
pubDatetime: 2022-03-25T16:55:12.000+00:00
slug: how-do-i-develop-my-portfolio-and-blog
featured: false
draft: false
tags:
  - NextJS
  - TailwindCSS
  - HeadlessCMS
  - Blog
description:
  "EXAMPLE POST: My experience about developing my first portfolio website and a blog
  using NextJS and a headless CMS."
timezone: "Asia/Yangon"
---

> 这篇文章最初来自我的[博客文章](https://satnaing.dev/blog/posts/how-do-i-develop-my-portfolio-and-blog)。我将此文放在这里，是为了演示如何使用 AstroPaper 主题撰写博客文章。

我使用 NextJS 和无头 CMS 开发第一个作品集网站和博客的经历。

![Building portfolio](https://satnaing.dev/_ipx/w_2048,q_75/https%3A%2F%2Fres.cloudinary.com%2Fnoezectz%2Fimage%2Fupload%2Fv1653050141%2FSatNaing%2Fblog_at_cafe_ei1wf4.jpg?url=https%3A%2F%2Fres.cloudinary.com%2Fnoezectz%2Fimage%2Fupload%2Fv1653050141%2FSatNaing%2Fblog_at_cafe_ei1wf4.jpg&w=2048&q=75)

## 动机

从我上大学时起，我就一直想着推出自己的网站并使用自己的域名（**satnaing.dev**）。但直到这个项目之前，这从未实现。我做过几个关于 Web 应用开发的项目和作品，但没有为此付出努力。

那么你可能会问：「那博客呢？」。是的，博客也一直是我项目清单上的一项。我一直想使用一些最新的技术来做一个博客项目。然而，我一直忙于工作和其他项目，所以博客项目从未启动。

这些日子里，我更倾向于以质量而非数量为核心来开发自己的项目。项目完成后，我通常会在 GitHub 仓库中放一份像样的 README 文件。但 GitHub 仓库的 README 只适合技术层面（这只是我的想法）。我想记录下我的经历和挑战。因此，我决定做自己的博客。而且，到了这个阶段，我已经有了相当多的经验和信心来完成这个项目。

## 技术栈

对于前端，我想使用 [React](https://reactjs.org/ "React Official Website")。但仅靠 React 对 SEO 不够友好；而且我还必须考虑路由、图片优化等许多因素。因此，我选择了 [NextJS](https://nextjs.org/ "NextJS Official Website") 作为我的主要前端框架。当然还有 TypeScript 用于类型检查。（据说一旦你习惯了 TypeScript，你就会爱上它 😉）

样式方面，我使用 [TailwindCSS](https://tailwindcss.com/ "Tailwind CSS Official Website")。这是因为我喜欢 Tailwind 提供的开发者体验，而且相比其他组件 UI 库（如 MUI 或 React Bootstrap），它具有很大的灵活性。

这个项目的所有内容都存放在 GitHub 仓库中。我的所有博客文章（包括这一篇）都是用 Markdown 文件格式编写的，因为我已经非常习惯这种格式。但为了轻松地编写 Markdown 及其 frontmatter，我使用了 [Forestry](https://forestry.io/ "Forestry Official Website") 无头 CMS。它是一个基于 Git 的 CMS，可以处理 Markdown 和其他内容。因此，我可以使用 Markdown 或所见即所得编辑器来编写内容。此外，用它来编写 frontmatter 简直轻而易举。

图片和资源上传并存储在 [Cloudinary](https://cloudinary.com/ "Cloudinary Official Website") 中。我通过 Forestry 连接 Cloudinary，并直接在后台管理它们。

总结来说，以下是我在这个项目中使用的技术栈：

- 前端：NextJS（TypeScript）
- 样式：TailwindCSS
- 动画：GSAP
- CMS：Forestry 无头 CMS
- 部署：Vercel

## 功能特性

以下是我的作品集和博客的一些功能特性：

### SEO 友好

整个项目以 SEO 为核心进行开发。我使用了适当的元标签、描述和标题层次。这个网站现在已经被 Google 收录了。

> 你可以通过搜索关键词「sat naing dev」在 Google 上找到这个网站

![searching satnaing.dev on google](https://res.cloudinary.com/noezectz/image/upload/v1648231400/SatNaing/satnaing-on-google_asflq6.png "satnaing.dev is indexed")

此外，由于正确使用了元标签，当分享到社交媒体时，这个网站也能很好地展示。

![satnaing.dev card layout when shared to Facebook](https://res.cloudinary.com/noezectz/image/upload/v1653106955/SatNaing/satnaing-dev-share-on-facebook_1_zjoehx.png "Card layout when shared to Facebook")

### 动态站点地图

站点地图在 SEO 中扮演着重要角色。因此，这个网站的每一页都应包含在 sitemap.xml 中。每当创建新的内容、标签或分类时，我的网站都会自动生成站点地图。

### 亮色与暗色主题

由于近年来暗色主题的流行趋势，如今许多网站都内置了暗色主题。当然，我的网站也支持亮色和暗色主题。

### 完全可访问

这个网站是完全可访问的。你只需使用键盘就能浏览整个网站。我遵循了所有 a11y 增强最佳实践，包括为所有图片添加 alt 文本、不跳过标题层级、使用语义化 HTML 标签、正确使用 aria 属性。

### 搜索框、分类与标签

所有博客内容都可以通过搜索框搜索。此外，内容还可以按分类和标签进行筛选。这样，博客读者可以搜索和阅读他们真正想要的内容。

### 性能与 Lighthouse 评分

得益于得当的开发和最佳实践，这个网站获得了非常好的性能和 Lighthouse 评分。以下是该网站的 Lighthouse 评分：

![satnaing.dev Lighthouse score](https://user-images.githubusercontent.com/53733092/159957822-7082e459-11e9-4616-8f1e-49d0881f7cbb.png "satnaing.dev Lighthouse score")

### 动画

最初我使用 [Framer Motion](https://www.framer.com/motion/ "Framer Motion") 来为网站添加动画和微交互。然而，当我尝试使用一些复杂的动画和视差效果时，我发现与 Framer Motion 的集成并不方便（也许我不是很擅长也不太习惯使用它）。因此，我决定使用 [GSAP](https://greensock.com/ "GSAP Animation Library") 来处理所有动画。它是最流行的动画库之一，能够实现复杂和高级的动画效果。你几乎可以在这个网站的每个页面上看到动画和微交互。

![animations at satnaing.dev](https://res.cloudinary.com/noezectz/image/upload/v1653108324/SatNaing/ezgif.com-gif-maker_2_hehtlm.gif "satnaing.dev website")

## 结语

总之，这个项目让我在开发博客网站（SSG）方面积累了很多经验和信心。现在，我已经掌握了基于 Git 的 CMS 及其如何与 NextJS 交互的知识。我还学习了 SEO、动态站点地图生成和 Google 收录流程。未来我会做出更好的项目。敬请期待！✌🏻

还有……最后但同样重要的是，我想对我的朋友 [Swann Fevian Kyaw](https://www.facebook.com/bon.zai.3910 "Swann Fevian Kyaw's Facebook Account")（@[ToonHa](https://www.facebook.com/ToonHa-102639465752883 "ToonHa Facebook Page")）说声「谢谢」，他为我网站的 Hero 区域绘制了一幅精美的插画。

## 项目链接

- 网站：[https://satnaing.dev/](https://satnaing.dev/ "https://satnaing.dev/")
- 博客：[https://satnaing.dev/blog](https://satnaing.dev/blog "https://satnaing.dev/blog")
- 仓库：[https://github.com/satnaing/my-portfolio](https://github.com/satnaing/my-portfolio "https://github.com/satnaing/my-portfolio")
