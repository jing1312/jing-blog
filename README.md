# jing-blog

我的个人博客，基于开源项目 [YYsuni/2025-blog-public](https://github.com/YYsuni/2025-blog-public)（MIT 协议）搭建。

**特点**：不写代码也能管理博客——部署之后，直接在网页上写文章、传图片、改配置，前端会自动提交到本仓库并触发重新部署。无后端、无数据库、无需服务器。

- 线上地址：https://jing-blog.15717985489.workers.dev （Cloudflare Workers）
- 内容仓库：本仓库 `public/blogs/` 目录（每篇文章一个文件夹）

## 技术栈

Next.js 16 + React 19 + Tailwind CSS 4。本仓库使用 **npm** 作为包管理器（OpenNext 打包在 Windows + pnpm 符号链接环境下有兼容问题，故切换为 npm）。

## 本地开发

```bash
npm install --legacy-peer-deps
npm run dev        # http://localhost:2025
```

## 部署（Cloudflare Workers，免费）

```bash
npx wrangler login          # 首次授权
npm run build:cf            # 构建（OpenNext）
npx wrangler deploy         # 部署
```

Cloudflare 账号 ID 已在部署命令中通过环境变量 `CLOUDFLARE_ACCOUNT_ID` 传入；站点地址等变量配置在 `wrangler.toml` 的 `[vars]`。

注意：Workers 免费版限制单脚本 3 MiB（gzip 后），为此 `src/lib/markdown-renderer.ts` 中的 shiki 已改为 core + 精选常用语言（30 种），未收录的语言会降级为纯代码块显示。

## 网页编辑功能（GitHub App）

1. GitHub → Settings → Developer Settings → New GitHub App：Webhook 关闭，权限只需 **Contents: Read and write**
2. 创建后生成 **Private Key**（自动下载的 `.pem`，只存在浏览器本地，勿上传），记下 **App ID**
3. Install 页面**只授权 jing-blog 仓库**
4. 打开网站 → 各页面右上角编辑按钮 → 按提示填入 App ID 与 Private Key

## 已知注意事项

- `*.workers.dev` 域名在大陆被 DNS 污染 + SNI 阻断，国内直连打不开；绑定自定义域名后即可正常访问（Workers 免费版支持自定义域名）。
- 网页端改动生效需等重新部署完成（约 1 分钟）再刷新。
- 文章点赞按钮连的是原项目作者的统计服务，如需独立统计需自行部署并修改 `src/components/like-button.tsx`。
