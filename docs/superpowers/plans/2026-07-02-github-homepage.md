# SII-k7.github.io 学术主页 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 按已批准规格（`docs/superpowers/specs/2026-07-02-github-homepage-design.md`）交付完整的暖调编辑风单页学术主页，本地目录 `SII-k7.github.io/`，用户推送后即为 GitHub Pages 站点。

**Architecture:** 纯静态单页——`index.html` + `css/style.css` + `assets/`（照片/论文图/占位 SVG）+ `favicon.svg` + `README.md`。零构建、零外部依赖、系统字体栈。响应式断点 900px / 700px / 480px。

**Tech Stack:** HTML5 + CSS3（custom properties），ffmpeg（图片裁切压缩），已勘察图源在 `.superpowers/pubsrc/`。

**验证方式:** 静态站点无单元测试——每个任务用「grep 断言 + 视觉检查（Read 截图/浏览器）」代替；隐私检查为硬性验收项。

**图源勘察结论（已完成）:**

| 论文 | 图源 | 处理 |
|---|---|---|
| RSS 2026 Active Surface | 无公开图 | SVG 占位 |
| T-Mech Mecanum | `.superpowers/pubsrc/logistic_gripper.png`（实验室主页 teaser，3751×2042，顶部有红字标题需裁掉） | 裁剪+压缩 |
| IJRR Force Sensing | `.superpowers/pubsrc/ijrr_fig-000.png`（arXiv 2506.11570 p4 机构图，2025×748，取上排 CAD 图） | 裁剪+压缩 |
| RSS 2024 Underactuated | `.superpowers/pubsrc/rss24_fig-000.png`（论文首页 teaser，916×505） | 直接压缩 |
| IROS 2024 Origami | `.superpowers/pubsrc/origami_teaser.jpg`（arXiv 2403.12471 首图，1362×774） | 直接压缩 |
| ReMAR 2024 3-PRS | 无公开图 | SVG 占位 |

**论文链接（已核实）:**
- T-Mech: `https://ieeexplore.ieee.org/document/11241047/`
- IJRR: `https://journals.sagepub.com/doi/10.1177/02783649251378155`
- RSS 2024: `https://www.roboticsproceedings.org/rss20/p101.html` + arXiv `https://arxiv.org/abs/2506.11570`
- IROS 2024: arXiv `https://arxiv.org/abs/2403.12471`
- RSS 2026 / ReMAR: 无公开链接，链接行省略（RSS 2026 显示 "preprint coming soon"）

---

### Task 1: 目录脚手架 + 个人照片 + favicon

**Files:**
- Create: `SII-k7.github.io/`（目录结构）
- Create: `SII-k7.github.io/assets/photo.jpg`
- Create: `SII-k7.github.io/favicon.svg`

- [ ] **Step 1: 建目录**

```bash
cd "/home/robot/zkq/github个人主页"
mkdir -p SII-k7.github.io/css SII-k7.github.io/assets/pubs
```

- [ ] **Step 2: 处理个人照片**（原图 2.8MB → 宽 700px、q3 高质量 JPEG，目标 ≤150KB）

```bash
cd "/home/robot/zkq/github个人主页"
ffmpeg -y -i "个人图片.jpg" -vf "scale=700:-1" -q:v 3 SII-k7.github.io/assets/photo.jpg
ls -la SII-k7.github.io/assets/photo.jpg   # 期望 < 200KB
```

- [ ] **Step 3: 写 favicon.svg**（赤陶底 KZ 字标）

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <rect width="64" height="64" rx="8" fill="#b3542e"/>
  <text x="32" y="44" font-family="Georgia, 'Times New Roman', serif" font-size="30" fill="#faf8f3" text-anchor="middle">KZ</text>
</svg>
```

- [ ] **Step 4: 验证** `file SII-k7.github.io/assets/photo.jpg` 输出 JPEG；Read favicon.svg 无误。

- [ ] **Step 5: Commit** `git add SII-k7.github.io && git commit -m "feat: scaffold site with photo and favicon"`

### Task 2: 论文配图（4 真图裁切压缩 + 2 SVG 占位）

**Files:**
- Create: `SII-k7.github.io/assets/pubs/{rss2026,tmech,ijrr,rss2024,iros2024,remar}.{jpg,svg}`

- [ ] **Step 1: 三张免裁图直接压缩**（统一目标宽 500px，JPEG q5，各 ≤120KB）

```bash
cd "/home/robot/zkq/github个人主页"
S=.superpowers/pubsrc; D=SII-k7.github.io/assets/pubs
ffmpeg -y -i $S/rss24_fig-000.png   -vf "scale=500:-1" -q:v 5 $D/rss2024.jpg
ffmpeg -y -i $S/origami_teaser.jpg  -vf "scale=500:-1" -q:v 5 $D/iros2024.jpg
```

- [ ] **Step 2: IJRR 图取上排 CAD 部分**（原图 2025×748，上排约占 55% 高度）

```bash
ffmpeg -y -i $S/ijrr_fig-000.png -vf "crop=2025:420:0:0,scale=500:-1" -q:v 5 $D/ijrr.jpg
```
Read 检查：应只含 A-1~A-5 CAD 渲染排，无下排公式图；裁高不合适就调 420→380/460 重跑。

- [ ] **Step 3: T-Mech 图裁掉红字标题区**（原图 3751×2042，标题约占顶部 12%；取中下部实验照片区）

```bash
ffmpeg -y -i $S/logistic_gripper.png -vf "crop=3751:1700:0:342,scale=500:-1" -q:v 5 $D/tmech.jpg
```
Read 检查：无红色中文标题、构图完整；不合适微调 crop 参数重跑。

- [ ] **Step 4: 两张 SVG 占位图**（500×300，米白底 + 赤陶描边 + 场馆缩写，风格统一）。`rss2026.svg`：

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 500 300">
  <rect width="500" height="300" fill="#f3efe6"/>
  <rect x="10" y="10" width="480" height="280" fill="none" stroke="#b3542e" stroke-width="1.5"/>
  <text x="250" y="140" font-family="Georgia, serif" font-size="42" fill="#b3542e" text-anchor="middle">RSS 2026</text>
  <text x="250" y="180" font-family="Helvetica, Arial, sans-serif" font-size="13" letter-spacing="3" fill="#8a8171" text-anchor="middle">FIGURE COMING SOON</text>
</svg>
```

`remar.svg` 同结构，将两处文本换成 `ReMAR 2024` / `FIGURE COMING SOON`。

- [ ] **Step 5: 验证尺寸** `du -h SII-k7.github.io/assets/pubs/*`，单文件 ≤200KB。

- [ ] **Step 6: Commit** `git commit -m "feat: add publication figures (4 real, 2 placeholders)"`

### Task 3: css/style.css（完整样式）

**Files:**
- Create: `SII-k7.github.io/css/style.css`

- [ ] **Step 1: 写完整样式文件。** 色板/字体 tokens 与三次批准的 mockup 完全一致：

```css
/* ============ tokens ============ */
:root {
  --bg: #faf8f3; --card: #fffdf9; --ink: #211d17; --body: #4a4438;
  --muted: #6e6656; --faint: #8a8171; --line: #e5ddcf; --line2: #d9d0be;
  --accent: #b3542e; --accent-soft: #d9a488; --peach: #e8a87c; --journal: #1a5b4f;
  --serif: Georgia, 'Times New Roman', 'Songti SC', 'SimSun', serif;
  --sans: Helvetica, Arial, 'PingFang SC', 'Microsoft YaHei', sans-serif;
}
* { margin: 0; padding: 0; box-sizing: border-box; }
html { scroll-behavior: smooth; }
body { background: var(--bg); color: var(--body); font-family: var(--serif); }
.wrap { max-width: 1180px; margin: 0 auto; padding: 0 64px; }
a { color: var(--accent); text-decoration: none; border-bottom: 1px solid var(--accent-soft); }
a:hover { color: var(--ink); border-bottom-color: var(--ink); }

/* ============ nav ============ */
nav { display: flex; justify-content: space-between; align-items: center;
      padding: 26px 64px; border-bottom: 1px solid var(--line); }
.logo { font-family: var(--sans); font-size: 14px; font-weight: 700; letter-spacing: 3px; color: var(--ink); }
.navlinks { font-family: var(--sans); font-size: 13px; display: flex; gap: 34px; }
.navlinks a { color: var(--muted); border-bottom: none; letter-spacing: 0.5px; }
.navlinks a:hover { color: var(--accent); }

/* ============ hero ============ */
.hero { display: flex; gap: 72px; padding: 88px 0 64px; align-items: flex-start; }
.hero-text { flex: 1.6; }
.eyebrow { font-family: var(--sans); font-size: 12px; letter-spacing: 5px; color: var(--accent); text-transform: uppercase; }
h1 { font-size: 72px; color: var(--ink); margin-top: 22px; line-height: 1.02; font-weight: 400; }
.cn-name { font-size: 26px; color: var(--faint); margin-top: 8px; }
.lede { font-size: 19px; line-height: 1.75; margin-top: 32px; max-width: 600px; }
.lede b { color: var(--ink); }
.sub { font-size: 16.5px; color: var(--muted); line-height: 1.75; margin-top: 16px; max-width: 600px; font-style: italic; }
.btns { display: flex; flex-wrap: wrap; gap: 16px; margin-top: 38px; font-family: var(--sans); font-size: 13.5px; }
.btns a { padding: 11px 26px; letter-spacing: 1.5px; border-bottom: none; }
.btn-solid { background: var(--ink); color: var(--bg); }
.btn-solid:hover { background: var(--accent); color: var(--bg); }
.btn-line { border: 1px solid var(--ink); color: var(--ink); }
.btn-line:hover { background: var(--ink); color: var(--bg); }
.btn-ghost { border: 1px solid var(--line2); color: var(--muted); }
.btn-ghost:hover { border-color: var(--ink); color: var(--ink); }
.hero-photo { flex: 1; max-width: 340px; }
.photo-wrap { position: relative; }
.photo-wrap::after { content: ''; position: absolute; top: 16px; left: 16px; right: -16px; bottom: -16px;
                     border: 1px solid var(--accent); }
.photo-wrap img { position: relative; z-index: 1; width: 100%; height: auto; display: block; filter: saturate(0.92); }
.photo-cap { font-family: var(--sans); font-size: 11px; color: var(--faint); margin-top: 32px;
             letter-spacing: 2px; text-align: right; }

/* ============ news ============ */
.news { padding-bottom: 24px; }
.news-inner { border-top: 2px solid var(--ink); padding-top: 22px; }
.news-items { margin-top: 16px; font-size: 15.5px; line-height: 2.2; }
.news-items .date { color: var(--accent); font-family: var(--sans); font-size: 13px; margin-right: 10px; }
.news-items b { color: var(--ink); }

/* ============ sections ============ */
.sec { padding: 64px 0 16px; }
.sec-head { display: flex; justify-content: space-between; align-items: baseline;
            border-top: 2px solid var(--ink); padding-top: 22px; }
.sec-label { font-family: var(--sans); font-size: 12px; letter-spacing: 4px; color: var(--ink); font-weight: 700; }
.sec-cn { font-size: 14px; color: var(--faint); }

/* ============ research ============ */
.research-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 28px; margin-top: 30px; }
.r-card { border: 1px solid var(--line); background: var(--card); padding: 30px 32px; position: relative; }
.r-card.now { border-color: var(--accent); }
.r-tag { position: absolute; top: -11px; left: 24px; background: var(--accent); color: var(--bg);
         font-family: var(--sans); font-size: 10px; letter-spacing: 2px; padding: 4px 12px; }
.r-tag.past { background: var(--faint); }
.r-card h3 { font-size: 22px; color: var(--ink); font-weight: 400; margin-top: 6px; }
.r-card .cn { font-size: 13px; color: var(--faint); margin-top: 4px; }
.r-card p { font-size: 14.5px; line-height: 1.8; margin-top: 14px; }
.r-kw { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 16px; }
.r-kw span { font-family: var(--sans); font-size: 11px; color: var(--muted);
             border: 1px solid var(--line2); padding: 4px 12px; border-radius: 14px; }

/* ============ publications ============ */
.pub { display: flex; gap: 32px; padding: 34px 0; border-bottom: 1px solid var(--line); }
.pub:last-of-type { border-bottom: none; }
.pub-img { width: 250px; flex-shrink: 0; align-self: flex-start; }
.pub-img img { width: 100%; height: 150px; object-fit: cover; display: block; border: 1px solid var(--line2); }
.pub-body { flex: 1; }
.pub-venue { display: inline-block; font-family: var(--sans); font-size: 11px; font-weight: 700;
             letter-spacing: 1.5px; color: var(--accent); border: 1px solid var(--accent); padding: 3px 10px; }
.pub-venue.journal { color: var(--journal); border-color: var(--journal); }
.pub-year { font-family: var(--sans); font-size: 11px; color: var(--faint); margin-left: 10px; letter-spacing: 1px; }
.pub h3 { font-size: 20px; color: var(--ink); font-weight: 400; line-height: 1.35; margin-top: 12px; }
.pub .authors { font-size: 14px; color: var(--muted); margin-top: 8px; line-height: 1.6; }
.pub .authors b { color: var(--ink); }
.pub .tldr { font-size: 14px; margin-top: 10px; line-height: 1.7; font-style: italic; }
.pub-links { display: flex; flex-wrap: wrap; gap: 18px; margin-top: 14px; font-family: var(--sans); font-size: 12px; }
.pub-links a { letter-spacing: 1px; padding-bottom: 1px; }
.pub-links .soon { color: var(--faint); letter-spacing: 1px; font-style: italic; }

/* ============ education timeline ============ */
.timeline { margin-top: 34px; position: relative; padding-left: 36px; }
.timeline::before { content: ''; position: absolute; left: 7px; top: 6px; bottom: 6px; width: 1px; background: var(--line2); }
.t-item { position: relative; padding-bottom: 38px; }
.t-item:last-child { padding-bottom: 8px; }
.t-item::before { content: ''; position: absolute; left: -33px; top: 7px; width: 9px; height: 9px;
                  border-radius: 50%; background: var(--bg); border: 2px solid var(--accent); }
.t-item.now::before { background: var(--accent); }
.t-date { font-family: var(--sans); font-size: 12px; letter-spacing: 2px; color: var(--accent); }
.t-item h3 { font-size: 21px; color: var(--ink); font-weight: 400; margin-top: 6px; }
.t-item .cn { font-size: 13.5px; color: var(--faint); margin-top: 3px; }
.t-item p { font-size: 14.5px; line-height: 1.75; margin-top: 8px; max-width: 720px; }
.t-item p b { color: var(--ink); }

/* ============ industry + honors ============ */
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 56px; }
.x-item { padding: 18px 0; }
.x-date { font-family: var(--sans); font-size: 11px; letter-spacing: 1.5px; color: var(--faint); }
.x-item h4 { font-size: 16.5px; color: var(--ink); font-weight: 400; margin-top: 5px; }
.x-item p { font-size: 13.5px; color: var(--muted); line-height: 1.65; margin-top: 5px; }
.x-item p b { color: var(--ink); }
.honor-list { margin-top: 12px; }
.honor-list li { font-size: 14.5px; line-height: 2.15; list-style: none; }
.honor-list li::before { content: '—'; color: var(--accent); margin-right: 12px; }

/* ============ misc ============ */
.misc { margin-top: 30px; }
details { border: 1px solid var(--line); background: var(--card); margin-bottom: 14px; }
summary { cursor: pointer; padding: 18px 26px; font-size: 16px; color: var(--ink); list-style: none;
          display: flex; justify-content: space-between; align-items: center; }
summary::-webkit-details-marker { display: none; }
summary::after { content: '+'; font-size: 22px; color: var(--accent); font-family: var(--sans); }
details[open] summary::after { content: '\2013'; }
summary .s-cn { font-size: 12.5px; color: var(--faint); margin-left: 12px; }
.d-body { padding: 0 26px 20px; font-size: 14.5px; line-height: 1.8; }

/* ============ footer ============ */
footer { margin-top: 72px; background: var(--ink); color: var(--faint); }
.f-inner { max-width: 1180px; margin: 0 auto; padding: 44px 64px;
           display: flex; justify-content: space-between; align-items: center; gap: 24px; flex-wrap: wrap; }
.f-name { font-size: 20px; color: var(--bg); }
.f-name .cn { font-size: 14px; color: var(--faint); margin-left: 10px; }
.f-copy { font-family: var(--sans); font-size: 11px; color: var(--muted); margin-top: 8px; }
.f-links { display: flex; gap: 24px; font-family: var(--sans); font-size: 12px; letter-spacing: 1.5px; }
.f-links a { color: var(--peach); border-bottom: none; }
.f-links a:hover { color: var(--bg); }

/* ============ responsive ============ */
@media (max-width: 900px) {
  .wrap { padding: 0 32px; }
  nav { padding: 20px 32px; }
  .navlinks { gap: 20px; }
  .hero { flex-direction: column-reverse; gap: 44px; padding-top: 48px; }
  .hero-photo { max-width: 260px; }
  h1 { font-size: 52px; }
  .f-inner { padding: 36px 32px; }
}
@media (max-width: 700px) {
  .research-grid, .two-col { grid-template-columns: 1fr; }
  .two-col { gap: 12px; }
  .pub { flex-direction: column; gap: 18px; }
  .pub-img { width: 100%; max-width: 340px; }
}
@media (max-width: 480px) {
  .wrap { padding: 0 20px; }
  nav { padding: 16px 20px; flex-direction: column; gap: 12px; }
  h1 { font-size: 40px; }
  .lede { font-size: 17px; }
  .btns a { padding: 10px 18px; }
  .f-inner { padding: 32px 20px; }
}
```

- [ ] **Step 2: Commit** `git commit -m "feat: add full stylesheet (warm editorial theme)"`

### Task 4: index.html（完整页面）

**Files:**
- Create: `SII-k7.github.io/index.html`

- [ ] **Step 1: 写完整 index.html。** 锚点映射按规格 §3.1（about/research/publications/experience/misc）。全部文案如下（TL;DR 已按检索到的摘要校准）：

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Keqi Zhu 朱科祺 — Robotics &amp; Embodied AI</title>
<meta name="description" content="Keqi Zhu (朱科祺) — Ph.D. student at Shanghai Innovation Institute working on RL-based whole-body control for humanoid robots. Previously: dexterous hands and robot mechanism design at Zhejiang University.">
<meta property="og:title" content="Keqi Zhu 朱科祺 — Robotics &amp; Embodied AI">
<meta property="og:description" content="RL-based humanoid whole-body control · dexterous hands · robot design. Ph.D. student at Shanghai Innovation Institute.">
<meta property="og:type" content="website">
<meta property="og:url" content="https://SII-k7.github.io/">
<meta property="og:image" content="https://SII-k7.github.io/assets/photo.jpg">
<link rel="icon" type="image/svg+xml" href="favicon.svg">
<link rel="stylesheet" href="css/style.css">
</head>
<body id="about">

<nav>
  <div class="logo">KEQI ZHU</div>
  <div class="navlinks">
    <a href="#about">About</a>
    <a href="#research">Research</a>
    <a href="#publications">Publications</a>
    <a href="#experience">Experience</a>
    <a href="#misc">Misc</a>
  </div>
</nav>

<div class="wrap">
  <header class="hero">
    <div class="hero-text">
      <div class="eyebrow">Robotics &middot; Embodied AI &middot; Reinforcement Learning</div>
      <h1>Keqi Zhu</h1>
      <div class="cn-name">朱科祺</div>
      <p class="lede">Ph.D. student at <a href="https://www.sii.edu.cn/" target="_blank" rel="noopener">Shanghai Innovation Institute</a>, working on <b>RL-based whole-body control for humanoid robots</b> &mdash; multi-contact locomotion and manipulation.</p>
      <p class="sub">Before diving into learning-based control, I spent years building the hardware itself &mdash; dexterous hands, grippers, and five generations of a humanoid robot. I build robots that move, and teach them to move well.</p>
      <div class="btns">
        <a class="btn-solid" href="https://scholar.google.com/citations?user=6d3LDPgAAAAJ" target="_blank" rel="noopener">Google Scholar</a>
        <a class="btn-line" href="https://github.com/SII-k7" target="_blank" rel="noopener">GitHub</a>
        <a class="btn-ghost" href="mailto:laozhu0626@gmail.com">Email</a>
      </div>
    </div>
    <div class="hero-photo">
      <div class="photo-wrap"><img src="assets/photo.jpg" alt="Keqi Zhu" width="700" height="1050"></div>
      <div class="photo-cap">SHANGHAI &middot; HANGZHOU</div>
    </div>
  </header>

  <section class="news">
    <div class="news-inner">
      <div class="sec-label">RECENT NEWS</div>
      <div class="news-items">
        <div><span class="date">2026</span> One paper accepted to <b>RSS 2026</b> &mdash; active surface-driven reconfigurable gripper for thin-object manipulation.</div>
        <div><span class="date">2025.09</span> Started my Ph.D. at Shanghai Innovation Institute &mdash; all in on humanoid RL. 开启人形机器人新篇章.</div>
        <div><span class="date">2025</span> Two journal papers out: <b>IEEE/ASME T-Mech</b> (in-hand Mecanum gripper) and <b>IJRR</b> (learning-based force sensing).</div>
      </div>
    </div>
  </section>

  <section class="sec" id="research">
    <div class="sec-head"><div class="sec-label">RESEARCH</div><div class="sec-cn">研究方向</div></div>
    <div class="research-grid">
      <article class="r-card now">
        <div class="r-tag">NOW &middot; 2025 &mdash;</div>
        <h3>Humanoid Whole-Body Control via RL</h3>
        <div class="cn">基于强化学习的人形机器人全身控制</div>
        <p>Reinforcement learning for humanoid whole-body control &mdash; multi-contact locomotion and loco-manipulation. Making humanoids exploit contacts with the world the way humans do, rather than avoiding them.</p>
        <div class="r-kw"><span>Reinforcement Learning</span><span>Whole-Body Control</span><span>Multi-Contact</span><span>Sim-to-Real</span></div>
      </article>
      <article class="r-card">
        <div class="r-tag past">FOUNDATION &middot; 2021 &mdash; 2025</div>
        <h3>Dexterous Hands &amp; Robot Design</h3>
        <div class="cn">灵巧手与机器人机构设计</div>
        <p>Mechanism design, modeling and control of dexterous grippers and novel robots &mdash; from underactuated force-sensing hands to in-hand manipulation with Mecanum wheels and origami robots.</p>
        <div class="r-kw"><span>Mechanism Design</span><span>Underactuated Grippers</span><span>In-hand Manipulation</span><span>Force Sensing</span></div>
      </article>
    </div>
  </section>

  <section class="sec" id="publications">
    <div class="sec-head"><div class="sec-label">SELECTED PUBLICATIONS</div><div class="sec-cn">论文</div></div>

    <article class="pub">
      <div class="pub-img"><img src="assets/pubs/rss2026.svg" alt="RSS 2026 — Active Surface-Driven Reconfigurable Gripper" width="500" height="300" loading="lazy"></div>
      <div class="pub-body">
        <span class="pub-venue">RSS 2026</span><span class="pub-year">ROBOTICS: SCIENCE AND SYSTEMS</span>
        <h3>Active Surface-Driven Reconfigurable Gripper: Robust Grasping and Sequential Manipulation of Thin Objects</h3>
        <div class="authors">Ziyi Zheng, <b>Keqi Zhu</b>, Hao Wu, Yanzhe Wang, Huixu Dong</div>
        <div class="tldr">TL;DR &mdash; A reconfigurable gripper whose active surfaces let it robustly pick up and sequentially manipulate thin, hard-to-grasp objects.</div>
        <div class="pub-links"><span class="soon">preprint coming soon</span></div>
      </div>
    </article>

    <article class="pub">
      <div class="pub-img"><img src="assets/pubs/tmech.jpg" alt="IEEE/ASME T-Mech — In-hand Gripper with Mecanum Wheel" width="500" height="300" loading="lazy"></div>
      <div class="pub-body">
        <span class="pub-venue journal">IEEE/ASME T-MECH</span><span class="pub-year">2025 &middot; JOURNAL</span>
        <h3>An In-Hand Gripper with Mecanum Wheel: Design, Modeling and Characterization</h3>
        <div class="authors"><b>Keqi Zhu</b>, Ziyi Zheng, Sihao Yang, Jituo Li, Huixu Dong</div>
        <div class="tldr">TL;DR &mdash; Mecanum and omni wheels embedded in a three-finger gripper act as active surfaces, enabling dexterous in-hand manipulation across five grasping configurations without complex control.</div>
        <div class="pub-links"><a href="https://ieeexplore.ieee.org/document/11241047/" target="_blank" rel="noopener">PAPER</a></div>
      </div>
    </article>

    <article class="pub">
      <div class="pub-img"><img src="assets/pubs/ijrr.jpg" alt="IJRR — Coupling-decoupling under-actuated gripper" width="500" height="300" loading="lazy"></div>
      <div class="pub-body">
        <span class="pub-venue journal">IJRR</span><span class="pub-year">2025 &middot; JOURNAL</span>
        <h3>Enabling to Learn for Force Sensing: A Coupling-Decoupling Under-actuated Gripper with Multiple-DoFs</h3>
        <div class="authors">Huixu Dong, Jihao Li, <b>Keqi Zhu</b>, Haotian Guo, Guodong Lu</div>
        <div class="tldr">TL;DR &mdash; Sensor-free force feedback from 0.1 N to 350 N: a hierarchical LSTM + mechanism model senses grasp forces purely through the actuator's current loop.</div>
        <div class="pub-links"><a href="https://journals.sagepub.com/doi/10.1177/02783649251378155" target="_blank" rel="noopener">PAPER</a></div>
      </div>
    </article>

    <article class="pub">
      <div class="pub-img"><img src="assets/pubs/rss2024.jpg" alt="RSS 2024 — Multiple-DOF underactuated gripper with force sensing" width="500" height="300" loading="lazy"></div>
      <div class="pub-body">
        <span class="pub-venue">RSS 2024</span><span class="pub-year">ROBOTICS: SCIENCE AND SYSTEMS</span>
        <h3>Construction of a Multiple-DOF Underactuated Gripper with Force-Sensing via Deep Learning</h3>
        <div class="authors">Jihao Li, <b>Keqi Zhu</b>, Guodong Lu, I-Ming Chen, Huixu Dong</div>
        <div class="tldr">TL;DR &mdash; A 2-finger 6-DoF underactuated gripper that switches between parallel and enveloping grasps, with LSTM-based force sensing and no force sensors &mdash; outperforming a Robotiq-85 baseline.</div>
        <div class="pub-links"><a href="https://www.roboticsproceedings.org/rss20/p101.html" target="_blank" rel="noopener">PAPER</a><a href="https://arxiv.org/abs/2506.11570" target="_blank" rel="noopener">ARXIV</a></div>
      </div>
    </article>

    <article class="pub">
      <div class="pub-img"><img src="assets/pubs/iros2024.jpg" alt="IROS 2024 — Multiple-locomotion origami robot" width="500" height="300" loading="lazy"></div>
      <div class="pub-body">
        <span class="pub-venue">IROS 2024</span><span class="pub-year">IEEE/RSJ INT. CONF. ON INTELLIGENT ROBOTS AND SYSTEMS</span>
        <h3>Theoretical Modeling and Bio-inspired Trajectory Optimization of A Multiple-locomotion Origami Robot</h3>
        <div class="authors"><b>Keqi Zhu</b>, Haotian Guo, Wei Yu, Hassen Nigatu Sirag, Tong Li, Ruihong Dong, Huixu Dong</div>
        <div class="tldr">TL;DR &mdash; An origami robot that both crawls and swims: friction-aware dynamics for crawling, plus a gait planner that finds human-like swimming strokes with maximum thrust.</div>
        <div class="pub-links"><a href="https://arxiv.org/abs/2403.12471" target="_blank" rel="noopener">ARXIV</a></div>
      </div>
    </article>

    <article class="pub">
      <div class="pub-img"><img src="assets/pubs/remar.svg" alt="ReMAR 2024 — Stiffness of 3-PRS parallel mechanism" width="500" height="300" loading="lazy"></div>
      <div class="pub-body">
        <span class="pub-venue">ReMAR 2024</span><span class="pub-year">INT. CONF. ON RECONFIGURABLE MECHANISMS AND ROBOTS</span>
        <h3>The Stiffness of 3-PRS PM Across Parasitic and Orientational Workspace</h3>
        <div class="authors">Hassen Nigatu, Jihao Li, <b>Keqi Zhu</b>, Junhan Zhang, Haotian Guo, Guodong Lu, Doik Kim</div>
        <div class="tldr">TL;DR &mdash; How parasitic motion shapes the stiffness of a 3-PRS parallel mechanism across its orientational workspace.</div>
        <div class="pub-links"></div>
      </div>
    </article>
  </section>

  <section class="sec" id="experience">
    <div class="sec-head"><div class="sec-label">EDUCATION</div><div class="sec-cn">求学经历</div></div>
    <div class="timeline">
      <div class="t-item now">
        <div class="t-date">2025.09 &mdash; PRESENT</div>
        <h3>Shanghai Innovation Institute &middot; Ph.D. in Artificial Intelligence</h3>
        <div class="cn">上海创智学院 &middot; 人工智能 &middot; 硕转博</div>
        <p>All in on <b>RL-based humanoid whole-body control</b> &mdash; multi-contact locomotion and manipulation.</p>
      </div>
      <div class="t-item">
        <div class="t-date">2023.09 &mdash; 2025.06</div>
        <h3>Zhejiang University &middot; M.S. in Mechanical Engineering</h3>
        <div class="cn">浙江大学 &middot; 机械工程</div>
        <p>Dexterous gripper design, modeling and control. Published at <b>RSS, IROS, T-Mech and IJRR</b>.</p>
      </div>
      <div class="t-item">
        <div class="t-date">2019.09 &mdash; 2023.06</div>
        <h3>Zhejiang University &middot; B.Eng. in Mechanical Engineering, Chu Kochen Honors College</h3>
        <div class="cn">浙江大学 竺可桢学院混合班 &middot; GPA 4.59/5 &middot; 专业前 5</div>
        <p>Foundation in control engineering, embedded systems and mechanics &mdash; <b>top-5 in major</b>, National Scholarship.</p>
      </div>
    </div>
  </section>

  <section class="sec">
    <div class="two-col">
      <div>
        <div class="sec-head"><div class="sec-label">INDUSTRY</div><div class="sec-cn">产业经历</div></div>
        <div class="x-item">
          <div class="x-date">2021.08 &mdash; 2025.12</div>
          <h4>Suishi Robotics &mdash; Lead Mechanical Designer</h4>
          <p>Led full mechanical design of a dual-arm humanoid robot through <b>five generations</b> &mdash; walking and object handover. Company acquired.</p>
        </div>
      </div>
      <div>
        <div class="sec-head"><div class="sec-label">SELECTED HONORS</div><div class="sec-cn">荣誉</div></div>
        <ul class="honor-list">
          <li>National Scholarship 国家奖学金</li>
          <li>ZJU Top-10 Outstanding Students 浙江大学&ldquo;十佳大学生&rdquo;</li>
          <li>Outstanding Graduate of Zhejiang Province 浙江省优秀毕业生</li>
          <li>National Silver Award, &ldquo;Jingdiao Cup&rdquo; Capstone Design 全国银奖</li>
          <li>First Prize, National Mathematics Competition 全国大学生数学竞赛一等奖</li>
        </ul>
      </div>
    </div>
  </section>

  <section class="sec" id="misc">
    <div class="sec-head"><div class="sec-label">MISC</div><div class="sec-cn">学术之外 &middot; 点击展开</div></div>
    <div class="misc">
      <details>
        <summary><span>&#129504; The Brain (最强大脑) &mdash; Seasons 8, 9 &amp; 10 <span class="s-cn">江苏卫视《最强大脑》三季选手</span></span></summary>
        <div class="d-body">Competed in three seasons of China's most-watched intelligence competition show &mdash; a different kind of problem-solving under pressure.</div>
      </details>
      <details>
        <summary><span>&#127891; Campus &amp; Community <span class="s-cn">校园与社群</span></span></summary>
        <div class="d-body">Student counselor &amp; class monitor, School of Mechanical Engineering &middot; Member of ASES (Asia-Pacific Student Entrepreneurship Society) &middot; Chen Xing Cultural China Talent Program, 13th cohort.</div>
      </details>
    </div>
  </section>
</div>

<footer>
  <div class="f-inner">
    <div>
      <div class="f-name">Keqi Zhu<span class="cn">朱科祺</span></div>
      <div class="f-copy">&copy; 2026 Keqi Zhu &middot; Built with plain HTML/CSS &middot; Hosted on GitHub Pages</div>
    </div>
    <div class="f-links">
      <a href="https://scholar.google.com/citations?user=6d3LDPgAAAAJ" target="_blank" rel="noopener">SCHOLAR</a>
      <a href="https://github.com/SII-k7" target="_blank" rel="noopener">GITHUB</a>
      <a href="mailto:laozhu0626@gmail.com">EMAIL</a>
    </div>
  </div>
</footer>

</body>
</html>
```

- [ ] **Step 2: 验证锚点** `grep -o 'id="[a-z]*"' SII-k7.github.io/index.html` 应输出 about/research/publications/experience/misc；`grep -c 'class="pub"' index.html` 应为 6。

- [ ] **Step 3: Commit** `git commit -m "feat: add complete single-page homepage"`

### Task 5: README.md（维护指南 + 发布步骤）

**Files:**
- Create: `SII-k7.github.io/README.md`

- [ ] **Step 1: 写 README**

````markdown
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
3. 仓库 Settings → Pages → Source 选 `main` 分支根目录（通常自动开启）
4. 几分钟后访问 https://sii-k7.github.io

## 如何加一篇新论文

打开 `index.html`，找到 `<section class="sec" id="publications">`，复制任意一个
`<article class="pub">…</article>` 块，粘贴到希望的位置，改六处：
图片路径 / 场馆徽章（期刊加 `journal` 类）/ 标题 / 作者（自己名字用 `<b>`）/ TL;DR / 链接。

配图放到 `assets/pubs/`，建议 500px 宽 JPEG（≤150KB）。

## 如何换论文占位图

RSS 2026 和 ReMAR 2024 目前是占位图（`assets/pubs/rss2026.svg`、`remar.svg`）。
拿到论文 teaser 图后压缩为 500px 宽 JPEG 放入 `assets/pubs/`，
并把 `index.html` 里对应 `<img src="assets/pubs/xxx.svg">` 改为新文件名。

## 色板

主色调定义在 `css/style.css` 顶部 `:root`，改一处全站生效。
````

- [ ] **Step 2: Commit** `git commit -m "docs: add README with publish and maintenance guide"`

### Task 6: 验证与验收

**Files:** 无新增（只读检查）

- [ ] **Step 1: 隐私硬检查**（必须全部 0 命中）

```bash
cd "/home/robot/zkq/github个人主页/SII-k7.github.io"
grep -rn "13777128953\|1156275661\|宁波\|党员\|24岁\|籍贯" . && echo "FAIL: privacy leak" || echo "PASS: no privacy info"
```

- [ ] **Step 2: 链接与资源完整性**

```bash
grep -o 'src="[^"]*"' index.html | sort -u        # 每个本地资源文件必须存在
grep -o 'href="[^"]*"' index.html | sort -u       # 外链人工核对一遍
ls assets/pubs/                                    # 应有 6 个文件
```

- [ ] **Step 3: 本地起服务，全页视觉检查**

```bash
cd "/home/robot/zkq/github个人主页/SII-k7.github.io" && python3 -m http.server 8899
```
访问 `http://localhost:8899`：对照三次批准的 mockup 检查桌面端；开发者工具切 375px 宽检查移动端（无横向滚动、无重叠）。

- [ ] **Step 4: 交付说明** — 告知用户：站点位置、待补图清单（RSS 2026 / ReMAR teaser）、推送步骤（README 里）、验收入口。

- [ ] **Step 5: Final commit** `git commit -am "chore: final verification pass"` （如有修正）

## Self-Review 记录

- **Spec coverage:** §3.1-3.8 全部板块 → Task 4；视觉系统 §2 → Task 3；图片策略 §3.4 → Task 2；隐私 §4 → Task 6 Step 1；文件结构 §5 → Task 1/5；README §7.6 → Task 5。无缺口。
- **Placeholder scan:** 无 TBD/TODO；所有代码块完整可执行；裁剪参数标注了「视觉检查后微调」属于明确的迭代步骤而非占位。
- **Type consistency:** CSS 类名与 HTML 使用一一对应（逐类核对过 nav/hero/news/sec/r-card/pub/timeline/two-col/misc/footer 系列）。
