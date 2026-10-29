# 部署

站点是纯静态产出：`pnpm build` 生成 `dist/`，丢给任何静态托管都能跑（Vercel / Cloudflare Pages / Cloudflare Workers / Netlify / GitHub Pages / 自己的服务器）。

## 产物

```
dist/
  index.html
  posts/<slug>/index.html     # trailingSlash: "always"，所以都有尾斜杠
  archive/  categories/  tags/  about/
  rss.xml  rss/  atom.xml  atom/
  sitemap-index.xml
  admin/publish/index.html    # 上传发布工具（不需要可以删掉这个路由）
  _astro/                     # 带 hash 的静态资源
  pagefind/                   # 搜索索引
```

## 通用设置

| 项 | 值 |
| --- | --- |
| 构建命令 | `pnpm build` |
| 安装命令 | `pnpm install` |
| 输出目录 | `dist` |
| Node 版本 | 见 `.nvmrc`（≥ 22） |

**换域名只改一处**：`src/config/siteConfig.ts` 的 `site_url`。canonical、sitemap、RSS 都从它派生。

> 构建时不需要任何环境变量；`.env.example` 里列出的都是可选开关。

## Cloudflare Workers（静态资源）

`wrangler.jsonc` 已经把 `dist/` 配成静态资源：

```jsonc
{
	"name": "sato-blog",
	"compatibility_date": "2026-10-19",
	"assets": {
		"directory": "./dist",
		"html_handling": "auto-trailing-slash",
		"not_found_handling": "404-page"
	}
}
```

`html_handling: "auto-trailing-slash"` 让 `/posts/hello-world` 自动跳到 `/posts/hello-world/` 并命中 `index.html`；`not_found_handling` 用导出的 404 页面。部署命令：

```bash
npx wrangler deploy
```

## Vercel / Netlify / Cloudflare Pages

不用额外配置，选框架预设 `Astro`、构建命令 `pnpm build`、输出目录 `dist` 即可。

`vercel.json` 里带了一组安全响应头（`X-Content-Type-Options`、`X-Frame-Options`、`Referrer-Policy`），Vercel 之外的平台会忽略这个文件，可以按需删掉或改成对应平台的配置。

## GitHub Actions（推送即检查）

`.github/workflows/ci.yml`：

1. `checks`：`pnpm install --frozen-lockfile` → `pnpm check` → `pnpm type-check` → `pnpm build` → 校验 `dist/` 关键文件存在；
2. `deploy-cf`（可选，靠 `vars.SITE_URL` 开关）：再跑一次 build，然后 `npx wrangler deploy`，并对线上做一轮冒烟检查。

启用 `deploy-cf` 需要在仓库 Settings → Secrets and variables → Actions 里配置：

| 名称 | 类型 | 值 |
| --- | --- | --- |
| `CLOUDFLARE_API_TOKEN` | Secret | Workers Scripts: Edit + Account Settings: Read |
| `CLOUDFLARE_ACCOUNT_ID` | Secret | Cloudflare 账户 ID |
| `SITE_URL` | Variable | 你的正式域名（同时是部署 job 的开关） |

不配 `SITE_URL` 时部署 job 会直接跳过，CI 只做检查，不会变红。

## 本地验证

```bash
pnpm build
pnpm preview
```

## 404 与尾斜杠

- `trailingSlash: "always"`：所有页面 URL 带尾斜杠，静态托管需要 `html_handling: "auto-trailing-slash"`（或 Netlify 的 `pretty_urls`）才能正确命中；
- 404 页是 `src/pages/404.astro` 导出的 `dist/404.html`，别删。
