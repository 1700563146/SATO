# Astro + Firefly 博客模板

一个开箱可用的纯静态个人博客模板，基于 [Astro 7](https://astro.build) + [Firefly](https://github.com/CuteLeaf/Firefly)（基于 [Fuwari](https://github.com/saicaca/fuwari)），带一批本站自研的功能（照片墙、音乐页、站内发布页、番茄钟、星际动效等）。

> 模板由 [Sato Blog](https://github.com/1700563146/Satone-blog) 整理而来：只保留代码、配置骨架、示例内容与占位资源，**作者本人的文章、照片、音乐、头像、域名和账号信息均已移除**。示例文章和配图都是模板自带的演示内容，可随意删除。

![tech](https://img.shields.io/badge/Astro-7-BC52EE) ![tech](https://img.shields.io/badge/Svelte-5-FF3E00) ![tech](https://img.shields.io/badge/Tailwind-4-38BDF8)

## 特性

- **内容**：Markdown / MDX 文章、草稿隔离、置顶、系列、文章密码、分类、标签、归档、全文搜索（Pagefind）
- **排版**：KaTeX 公式、Mermaid 图表、PlantUML、代码块行号 / 折叠 / 语言徽章、GitHub 卡片、提示框、图片网格
- **界面**：亮 / 暗色主题、360° 主题色、四种壁纸模式、双侧栏布局、侧栏组件自由排序（资料 / 公告 / 音乐 / 分类 / 标签 / 统计 / 日历 / 番茄钟 / 目录 / 广告位）
- **页面**：首页、文章列表、文章详情、归档、分类、标签、系列、关于、留言板、友链、动态（微语）、项目、相册、照片墙、音乐、书签导航、B 站 / 番组 / VNDB / MAL、打赏 —— 每一项都能在 `src/config/siteConfig.ts` 的 `pages` 里开关
- **交互**：Swup 无刷新跳转、星空 / 视差 / 指针光晕 / 磁吸按钮 / 阅读进度、樱花飘落、PWA 图标与 manifest
- **发布**：`pnpm new-post`、站内 `/admin/publish/`（浏览器直连 GitHub API 提交）、Pages CMS（`.pages.yml`）
- **部署**：纯静态 `dist/`，Vercel / Cloudflare / Netlify / GitHub Pages 均可；自带 GitHub Actions 检查与可选部署

## 快速开始

```bash
# 需要 Node >= 22 与 pnpm（仓库用 pnpm-lock.yaml 锁定版本）
corepack enable
pnpm install

pnpm dev        # http://localhost:4321
```

```bash
pnpm build      # 生产构建 → dist/（LQIP → 图标 → Astro → 字体子集 → Pagefind）
pnpm preview    # 预览构建产物
pnpm check      # Astro 诊断
pnpm type-check # TypeScript 检查
pnpm format     # Biome 格式化
```

## 改成你自己的站点

改配置就够了，基本不用碰组件。建议按这个顺序：

1. **站点信息** `src/config/siteConfig.ts`
   - `title` / `subtitle` / `description` / `keywords`
   - `site_url`：换成你的域名（canonical、sitemap、RSS 都从它派生）
   - `siteStartDate`、`timezone`、`themeColor.hue`、`pageWidth`
   - `pages`: 想开哪些页面就把对应项改成 `true`
2. **个人信息** `src/config/profileConfig.ts`：头像、名字、签名、社交链接
3. **首屏** `src/config/backgroundWallpaper.ts`：壁纸图片、横幅标题与副标题、打字机效果、背景视频
4. **导航与侧栏** `src/config/navBarConfig.ts`、`src/config/sidebarConfig.ts`
5. **占位资源**：把 `src/assets/images/avatar.avif`、`src/assets/images/DesktopWallpaper/d1.avif`、
   `src/assets/images/MobileWallpaper/m1.avif`、`public/favicon/*` 换成自己的图片
6. **示例内容**：`src/content/posts/`、`src/content/spec/`、`src/content/dynamic/`、`src/content/projects/`
7. **仓库地址**：站内发布页要往你的仓库提交，改 `src/pages/admin/publish.astro` 顶部的 `PUBLISH_REPO`

仓库里需要替换的占位符（全仓库搜索即可）：

| 占位符 | 出现位置 |
| --- | --- |
| `your-github-name` / `your-name` | `src/config/*.ts`、`src/pages/admin/publish.astro`、`src/content/spec/friends.mdx` |
| `your-blog-repo` | `src/pages/admin/publish.astro` |
| `example.com` / `you@example.com` | `src/config/siteConfig.ts`、`backgroundWallpaper.ts`、`friends.mdx` |
| `Astro Blog`（站点标题） | `src/config/siteConfig.ts`、`src/config/backgroundWallpaper.ts` |

## 写文章

文章 = `src/content/posts/<slug>.md`，文件名就是 URL：`hello-world.md` → `/posts/hello-world/`。

```yaml
---
title: "文章标题"
published: 2026-10-01
description: "列表与 RSS 里显示的摘要"
tags: ["标签一", "标签二"]
category: "分类"
draft: false
pinned: false
---
```

三种写法，产物都是同一个目录里的 `.md`：

1. **本地**：`pnpm new-post my-slug` 然后 `pnpm dev` 预览，`git push` 上线；
2. **站内发布页**：`/admin/publish/`（右下角「写文章」按钮或 `Ctrl/⌘+Shift+P`），拖入 Markdown 提交到仓库；
3. **Pages CMS**：<https://app.pagescms.org> 读根目录 `.pages.yml`，给仓库装 GitHub App 后即可在线编辑。

细节见 [内容规范](docs/CONTENT.md)、[发文章](docs/PUBLISH.md)。

## 目录速览

```
src/config/          所有配置（标题、导航、侧栏、壁纸、评论、字体…）
src/content/posts/    文章            src/content/spec/      关于 / 留言板 等特殊页
src/pages/            路由            src/components/        组件（按域分组）
src/layouts/          页面骨架        src/plugins/           remark / rehype 插件
src/i18n/             多语言          src/styles/            Tailwind 4 与全局样式
scripts/              构建期脚本（LQIP、字体子集、Pagefind、曲库同步…）
public/               原样复制到 dist（图片、字体、favicon、音乐、照片墙）
docs/                 中文文档
```

## 文档

- [内容规范](docs/CONTENT.md) — frontmatter 字段与图片规则
- [发文章](docs/PUBLISH.md) — 站内发布页 / Pages CMS / 本地三条路
- [深度定制](docs/CUSTOMIZE.md) — 配置速查、外观、组件级修改、升级上游
- [部署](docs/DEPLOYMENT.md) — 静态产物、Cloudflare / Vercel / Actions
- [照片墙](docs/PHOTO-WALL.md) — 照片格式与体积
- [音乐页](docs/MUSIC.md) — 接口选型、曲库同步与离线化
- [番茄钟](docs/POMODORO.md) — 占位组件的现状与接入计划
- 主题上游文档：<https://docs-firefly.cuteleaf.cn/>

## 说明

- 评论、统计、B 站 / 番组 / VNDB / MAL、打赏等需要外部服务或账号的功能默认关闭，配置项都在 `src/config/`，用到再开；
- 模板不内置任何第三方音频、视频、游戏插画或他人照片：`public/music/` 只有一份空的 `library.json`，
  `public/wall/demo/` 里的 9 张示例图是程序生成的渐变图；
- 站点图标、字体、`src/assets/images/avatar.avif` 等来自 Firefly 主题自带资源。

## 致谢与许可

- [Astro](https://astro.build)、[Fuwari](https://github.com/saicaca/fuwari)（MIT）、[Firefly](https://github.com/CuteLeaf/Firefly)（MIT）
- 本仓库同样以 [MIT](LICENSE) 发布，保留上游版权声明
