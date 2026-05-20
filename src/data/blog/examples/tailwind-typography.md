---
title: Tailwind Typography Plugin
author: Sat Naing
pubDatetime: 2022-07-05T02:05:51Z
featured: false
draft: false
tags:
  - TypeScript
  - Astro
description: "EXAMPLE POST: About Tailwind Typography Plugin and how you can use it effectively."
---

> 这篇文章来自 [TailwindLabs](https://tailwindcss-typography.vercel.app/)。我将此文放在这里，是为了演示如何使用 AstroPaper 主题撰写博客文章。

默认情况下，Tailwind 会移除段落、标题、列表等的所有默认浏览器样式。这对于构建应用 UI 来说非常有用，因为你花在撤销用户代理样式上的时间更少，但当你确实只是想为来自 CMS 或 Markdown 文件的富文本编辑器中的内容设置样式时，它会令人惊讶和不直观。

我们实际上收到了很多关于此问题的抱怨，经常有人问我们这样的问题：

> 为什么 Tailwind 移除了我的 `h1` 元素上的默认样式？我该如何禁用它？你说我还会失去所有其他基础样式是什么意思？
> 我们听到了你的声音，但我们不认为仅仅禁用我们的基础样式是你真正想要的。你不想每次在仪表板 UI 中使用 `p` 元素时都要移除烦人的边距。而且我也怀疑你真的希望你的博客文章使用用户代理样式——你希望它们看起来很棒，而不是很难看。

`@tailwindcss/typography` 插件是我们为你提供你真正想要的东西的尝试，而没有任何像禁用基础样式这样愚蠢的做法所带来的负面影响。

它添加了一个新的 `prose` 类，你可以将其应用于任何原生 HTML 内容块，并将其变成一篇排版精美、格式良好的文档：

```html
<article class="prose">
  <h1>Garlic bread with cheese: What the science tells us</h1>
  <p>
    For years parents have espoused the health benefits of eating garlic bread
    with cheese to their children, with the food earning such an iconic status
    in our culture that kids will often dress up as warm, cheesy loaf for
    Halloween.
  </p>
  <p>
    But a recent study shows that the celebrated appetizer may be linked to a
    series of rabies cases springing up around the country.
  </p>
  <!-- ... -->
</article>
```

有关如何使用该插件及其包含的功能的更多信息，请[阅读文档](https://github.com/tailwindcss/typography/blob/master/README.md)。

---

## 接下来你将看到什么

接下来只是一大堆我写的废话，用来自己测试这个插件本身。它涵盖了我能想到的每一种合理的排版元素，比如**粗体文本**、无序列表、有序列表、代码块、块引用，*甚至斜体*。

覆盖所有这些用例很重要，原因如下：

1. 我们希望一切在开箱即用时看起来都很好。
2. 其实只有第一个原因，这才是这个插件的全部意义所在。
3. 不过这里有一个第三个假装的理由，因为有三个项目的列表比有两个项目的列表看起来更真实。

现在我们要尝试另一种标题样式。

### 排版应该很简单

所以那是一个给你的标题——如果幸运的话，我们的工作做得正确，它看起来会相当合理。

一个智者曾经告诉我关于排版的事情是：

> 如果你不想让你的东西看起来像废话，排版就非常重要。

也许没那么优美，但确实是真的。

当然，人们不总是在谈论为什么排版很重要之类的东西。

---

## 代码应该看起来像代码

这很容易做到，而且 Tailwind 在这方面做得很好。它只是使用等宽字体。

```js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;
const divide = (a, b) => a / b;
console.log(add(2, 2));
console.log(subtract(2, 2));
console.log(multiply(2, 2));
console.log(divide(2, 2));
```

然而，我们仍然需要确保行内代码看起来很合理，就像我想告诉你关于 `<span>` 元素或 `@tailwindcss/typography` 的好消息那样。

---

## 列表

由于 Markdown 可以生成很多列表，我们需要确保它们看起来不错。

- 这是一个无序列表。
- 它有两项。
- 但这里有三项了。

### 有序列表

1. 这是一项。
2. 那是另一项。
3. 你猜对了，第三项。

---

## 我们甚至为嵌套列表提供了样式

很多人以为嵌套列表是糟糕的 HTML，但他们错了。

1. **关于列表有很多话要说。**
   - 我不确定我们是否会费心去为超过两层的嵌套设置样式。
   - 两层已经太多了，三层肯定是馊主意。
   - 如果你嵌套四层，你就该进监狱了。
2. **只有两项不算是一个真正的列表，不过三个就很好。**
   - 再说一遍，如果你想让人真正阅读你的内容，请不要嵌套列表。
   - 没有人想看这个。
   - 我很沮丧我们甚至不得不为此设置样式。

Markdown 中关于列表最烦人的事情是，`<li>` 元素不会被赋予子 `<p>` 标签，除非该列表项中有多个段落。这意味着我也必须担心为那种烦人的情况设置样式。

- **例如，这里是另一个嵌套列表。**

  但这次有第二段。
  - 这些列表项不会有 `<p>` 标签
  - 因为它们每项只有一行

- **但在这个第二层顶级列表项中，它们会有。**

  这尤其烦人，因为这段的间距问题。
  - 正如你在这里看到的，因为我添加了第二行，这个列表项现在有了 `<p>` 标签。

    这就是我说的第二行。

  - 最后这里是另一个列表项，这样看起来更像一个列表。

- 一个收尾的列表项，但没有嵌套列表，为什么不呢？

最后用一句话来结束这一节。

## 还有其他元素需要设置样式

我差点忘了提到链接，比如[这个指向 Tailwind CSS 网站的链接](https://tailwindcss.com)。我们差点把它们做成蓝色的，但那太老土了，所以我们用了深灰色，感觉更前卫。

我们甚至还包括了表格样式，看看吧：

| Wrestler                | Origin       | Finisher           |
| ----------------------- | ------------ | ------------------ |
| Bret "The Hitman" Hart  | Calgary, AB  | Sharpshooter       |
| Stone Cold Steve Austin | Austin, TX   | Stone Cold Stunner |
| Randy Savage            | Sarasota, FL | Elbow Drop         |
| Vader                   | Boulder, CO  | Vader Bomb         |
| Razor Ramon             | Chuluota, FL | Razor's Edge       |

我们还需要确保行内代码看起来不错，比如我想谈论 `<span>` 元素或告诉你关于 `@tailwindcss/typography` 的好消息。

### 有时候我甚至在标题中使用 `code`

尽管这可能是个坏主意，而且历史上我很难让它看起来好看。不过这个「把代码块包在反引号里」的技巧确实相当好用。

我过去做的另一件事是把 `code` 标签放在链接里面，比如我想告诉你关于 [`tailwindcss/docs`](https://github.com/tailwindcss/docs) 仓库。我不太喜欢反引号下面有下划线，但避免它所需付出的疯狂代价绝对不值得。

#### 我们还没用过 `h4`

但现在我们用了。请不要在你的内容中使用 `h5` 或 `h6`，Medium 只支持两个标题层级是有原因的，你们这些家伙。老实说，我曾考虑过使用 `before` 伪元素，在你使用 `h5` 或 `h6` 时对你尖叫。

我们默认完全不设置它们的样式，因为 `h4` 元素已经很小了，它们跟正文一样大。我们还能对 `h5` 做什么，把它变得比正文还小？不了，谢谢。

### 不过我们仍然需要考虑堆叠标题的问题。

#### 让我们确保也不会搞砸 `h4` 元素。

呼，幸运的话，我们已经为这段文字上方的标题设置了样式，它们看起来应该还不错。

让我们在这里添加一个收尾段落，这样内容以一段大小合适的文本块结束。我无法解释为什么我希望以这种方式结束，但我猜是因为我觉得如果标题离文档末尾太近，会看起来很奇怪或不平衡。

我在这里写的应该已经够长了，但加上这最后一句话也无妨。
