# jing-blog

我的个人博客，基于开源项目 [YYsuni/2025-blog-public](https://github.com/YYsuni/2025-blog-public)（MIT 协议）搭建。

**特点**：不写代码也能管理博客——部署之后，直接在网页上写文章、传图片、改配置，前端会自动提交到本仓库并触发重新部署。无后端、无数据库、无需服务器。

## 技术栈

Next.js 16 + React 19 + Tailwind CSS 4，内容全部存储在 `public/blogs/` 目录（每篇文章一个文件夹），通过 GitHub + Vercel 免费托管。

## 本地开发

```bash
pnpm install
pnpm dev        # http://localhost:2025
```

## 部署步骤

1. **部署到 Vercel**：<https://vercel.com> 导入本仓库，什么都不用配，直接 Deploy。
2. **创建 GitHub App**：GitHub → Settings → Developer Settings → New GitHub App
   - Webhook 关闭；权限只需 **Contents: Read and write**
   - 创建后生成 **Private Key**（自动下载，妥善保管）并记下 **App ID**
   - 到 Install 页面，**只授权 jing-blog 这个仓库**
3. **配置环境变量**（Vercel 项目 → Settings → Environment Variables，然后手动 Redeploy 一次）：
   - `NEXT_PUBLIC_GITHUB_APP_ID` = 你的 App ID
   - `NEXT_PUBLIC_GITHUB_OWNER` = `jing1312`（已写死在 `src/consts.ts`，可不设）
   - `NEXT_PUBLIC_GITHUB_REPO` = `jing-blog`（已写死，可不设）
   - `NEXT_PUBLIC_SITE_URL` = 部署后的网址（用于 RSS）
4. **开始使用**：打开网站，首页右上角有配置按钮；各页面右上角的编辑按钮配合 Private Key 即可在网页端写作和管理。

## 已知注意事项

- 文章点赞按钮连的是原项目作者的统计服务，如需独立统计需要自己部署后端并修改 `src/components/like-button.tsx`。
- 网页端改动生效需要等 Vercel 部署完成（约 1 分钟）再刷新。
- Private Key 只保存在自己浏览器本地（加密存储），不要提交到仓库。
