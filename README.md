# SII-k7.github.io

Keqi Zhu (朱科祺) 的个人学术主页 — 纯静态 HTML/CSS，无构建步骤。

## 发布到 GitHub Pages

1. 在 GitHub 上创建仓库，名字必须是 `SII-k7.github.io`（公开）
2. 在本目录执行：
   ```bash
   git init && git add . && git commit -m "initial homepage"
   git branch -M main
   git remote add origin git@github.com:SII-k7/SII-k7.github.io.git
   git push -u origin main
   ```
   （如果用 HTTPS：`git remote add origin https://github.com/SII-k7/SII-k7.github.io.git`）
3. 仓库 Settings → Pages → Source 选 `main` 分支根目录（通常自动开启）
4. 几分钟后访问 https://sii-k7.github.io

## 如何加一篇新论文

打开 `index.html`，找到 `<section class="sec" id="publications">`，复制任意一个
`<article class="pub">…</article>` 块，粘贴到希望的位置，改六处：

1. 图片路径 `assets/pubs/xxx.jpg`
2. 场馆徽章 `<span class="pub-venue">`（期刊加 `journal` 类变墨绿色）
3. 标题
4. 作者行（自己名字用 `<b>Keqi Zhu</b>`）
5. TL;DR 一句话
6. 链接行（PAPER / ARXIV / VIDEO / PROJECT）

配图放到 `assets/pubs/`，建议 500px 宽 JPEG（≤150KB）。压缩命令：
```bash
ffmpeg -i 原图.png -vf "scale=500:-1" -q:v 5 assets/pubs/新论文.jpg
```

## 待补的论文占位图

- `assets/pubs/rss2026.svg` — RSS 2026 论文（拿到 camera-ready teaser 后替换）
- `assets/pubs/remar.svg` — ReMAR 2024 论文

拿到图后压缩为 500px 宽 JPEG 放入 `assets/pubs/`，
并把 `index.html` 里对应 `<img src="assets/pubs/xxx.svg">` 改为新文件名。
RSS 2026 发布 arXiv 后，把 `preprint coming soon` 那行换成
`<a href="arXiv链接" target="_blank" rel="noopener">ARXIV</a>`。

## 改 News

`<section class="news">` 里每条 `<div><span class="date">…</span> …</div>` 是一条新闻，
新的放最上面，保持 2-4 条。

## 色板

主色调定义在 `css/style.css` 顶部 `:root`，改一处全站生效：
- `--accent: #b3542e` 赤陶橙（点缀色）
- `--bg: #faf8f3` 米白背景
- `--ink: #211d17` 深炭黑
