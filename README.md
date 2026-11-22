# nw-village.com 站点（Hugo 双语）

一个零主题依赖的极简双语站：中文在根路径，英文在 `/en/`。
已在本机用 Hugo 0.170.1 构建验证通过（ZH 12 页 / EN 11 页，无警告）。

## 目录结构

```
nw-village-site/
├── hugo.toml              # 站点配置（双语、菜单、描述）
├── layouts/
│   ├── index.html         # 首页（简介 + 最新文章列表）
│   └── _default/
│       ├── baseof.html    # 全站骨架（内嵌 CSS，米纸底色/衬线字体）
│       ├── list.html      # 列表页（/posts/）
│       └── single.html    # 文章页
├── content/
│   ├── _index.zh.md / _index.en.md    # 首页简介
│   ├── about.zh.md   / about.en.md    # 关于页
│   └── posts/
│       └── hello-world.zh.md / hello-world.en.md   # 第一篇文章（中英）
└── .gitignore             # 排除 public/ 构建产物
```

## 本地预览

```bash
hugo server -D
# 浏览器打开 http://localhost:1313 （英文版 http://localhost:1313/en/）
```

## 写新文章

1. 在 `content/posts/` 新建 `2026-12-01-my-story.zh.md`，头部写：
   ```yaml
   ---
   title: "文章标题"
   description: "一句话摘要（列表页显示）"
   date: 2026-12-01
   translationKey: "my-story"     # 与英文版相同，自动互相关联
   ---
   ```
2. 英文版同名加 `.en.md` 后缀，`translationKey` 保持一致。
3. 保存后 `hugo server` 会自动刷新预览。

## 发布（Cloudflare Pages + GitHub）

1. GitHub 上新建仓库 `nw-village`（Public），**不要**勾选自动生成 README；
2. 在本项目目录执行：
   ```bash
   git init
   git add .
   git commit -m "init: hugo bilingual site"
   git branch -M main
   git remote add origin https://github.com/WnagBaobao/nw-village.git
   git push -u origin main
   ```
3. 回到 Cloudflare Pages「Select a repository」页面 → 选中 `nw-village` → Next；
4. 构建设置填：
   - **Framework preset**: `Hugo`
   - **Build command**: `hugo --gc --minify`
   - **Build output directory**: `public`
   - **环境变量（Variables and Secrets）**：`HUGO_VERSION` = `0.170.1`
     （重要：Pages 不设版本会用极旧的 Hugo，双语 label 语法会报错）
5. 点 Save and Deploy，几分钟后得到 `xxx.pages.dev` 预览地址；
6. 项目 → **Custom domains** → 添加 `nw-village.com` 和 `www.nw-village.com`（DNS 已在 Cloudflare，会自动加 CNAME）；
7. 等 SSL 激活（几分钟）→ 完成。

以后每次写完文章：`git add . && git commit -m "post: 标题" && git push` → 自动重新构建发布。
