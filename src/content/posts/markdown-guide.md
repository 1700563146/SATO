---
title: "Markdown 写法示例"
published: 2026-09-20
description: "把主题支持的常见排版都放在一篇文章里：标题、列表、表格、代码块、公式、提示框、任务列表。"
tags: ["Markdown", "写作"]
category: "写作指南"
---

这篇文章是排版示例，写新文章前可以打开对照一下。整篇内容都在 `src/content/posts/markdown-guide.md`，需要时直接删掉。

## 标题与段落

正文里的**加粗**、*斜体*、~~删除线~~、`行内代码` 和 [链接](https://docs.astro.build/) 都是标准 Markdown 写法。段落之间空一行即可。

## 列表

- 无序列表第一项
- 无序列表第二项
  - 可以嵌套
  - 缩进两个空格

1. 有序列表
2. 第二项
3. 第三项

任务列表：

- [x] 已经做完的事
- [ ] 还没做的事

## 表格

| 语法 | 说明 |
| --- | --- |
| `**加粗**` | 强调 |
| `` `代码` `` | 行内代码 |
| `> 引用` | 引用块 |

## 代码块

代码块会显示语言徽章，行数超过 15 行时自动折叠：

```ts
type Post = {
	title: string;
	published: Date;
	tags?: string[];
};

export function formatPost(post: Post): string {
	const tags = post.tags?.join(" / ") ?? "无标签";
	return `${post.title} · ${tags}`;
}
```

## 公式

行内公式：$E = mc^2$；独立公式：

$$
\int_{-\infty}^{\infty} e^{-x^2}\,\mathrm{d}x = \sqrt{\pi}
$$

## 提示框

> [!NOTE]
> 这是 note 提示框，正文里写 `> [!NOTE]` 即可。

> [!TIP]
> 还可以用 `TIP` / `IMPORTANT` / `WARNING` / `CAUTION`。

> [!WARNING]
> 改 `src/config/` 里的配置后，记得重启 `pnpm dev` 才会生效。

## 引用

> 把想法写下来，是检验自己有没有真的想明白的最快方式。

## 图片

图片放 `src/content/posts/images/`，正文用相对路径引用，构建时会自动转 webp、补宽高并懒加载：

```md
![图片说明](./images/example.png)
```
