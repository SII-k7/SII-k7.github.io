# SII-k7.github.io 个人学术主页 — 设计规格

日期：2026-07-02
状态：已获用户逐节批准（首屏 / Research+Publications / Education+Misc 三节均通过视觉伴侣确认）
用户委托：规格自审通过后直接执行，无需再次送审；最终整站交付验收。

## 1. 目标与范围

为朱科祺（Keqi Zhu，GitHub 用户名 **SII-k7**）搭建 GitHub Pages 个人学术主页：

- 仓库：`SII-k7.github.io`（用户自行创建并推送，本地先完成全部文件）
- 形态：**单页滚动**学术主页 + 顶部锚点导航
- 技术：**纯手写静态 HTML/CSS**，零构建、零 JS 依赖（仅允许少量原生 JS 做平滑滚动增强，非必需）
- 语言：**英文主体 + 中文点缀**（姓名、板块中文小字、个别短语）
- 核心叙事：从灵巧手/机构设计（硬件根基）→ 人形机器人 RL 全身控制（当前方向）；学术为核心，其他内容弱化

不做：CV 下载、多页站点、深浅色切换、构建工具链、评论/统计等动态功能。

## 2. 视觉系统（已批准：暖调编辑风）

| 元素 | 值 |
|---|---|
| 背景 | `#faf8f3` 米白暖底；卡片 `#fffdf9` |
| 正文字色 | `#4a4438`；标题 `#211d17`；次要 `#6e6656` / `#8a8171` |
| 强调色 | 赤陶橙 `#b3542e`（眉标、徽章、时间线节点、链接）；辅助 `#e8a87c`（页脚链接） |
| 期刊徽章 | 墨绿 `#1a5b4f`（与会议赤陶色区分） |
| 分隔线 | `#e5ddcf` 细线；板块顶部 `2px solid #211d17` 粗线 |
| 标题字体 | Georgia / 衬线栈（大号、font-weight 400） |
| 标签/按钮字体 | Helvetica / 无衬线栈，小号大写 + letter-spacing |
| 页脚 | 深炭黑 `#211d17` 反白 |
| 内容宽度 | max-width 1180px，两侧 padding 64px（移动端收窄） |

签名元素：照片赤陶色偏移描边框（top/left +16px 错位）；板块头部「粗黑线 + 大写字距标签 + 右侧中文小字」。

## 3. 页面结构（单页，自上而下）

### 3.1 导航栏
左：`KEQI ZHU` 字标；右：About / Research / Publications / Experience / Misc 锚点链接，当前悬停赤陶色。sticky 可选（默认不 sticky，保持安静）。

### 3.2 Hero（已批准 v2 全宽版）
- 左列：眉标 `ROBOTICS · EMBODIED AI · REINFORCEMENT LEARNING`；72px 衬线 `Keqi Zhu` + 中文名；主文案（Ph.D. at Shanghai Innovation Institute, RL-based humanoid WBC — multi-contact locomotion and manipulation）；斜体副文案（硬件→学习的转型叙事，含 "I build robots that move, and teach them to move well."）；三按钮：Google Scholar（实心）/ GitHub / Email（描边）
- 右列：`个人图片.jpg` 裁切优化后的照片 + 偏移描边，下方 `SHANGHAI · HANGZHOU` 小字
- Recent News 条：2026.01 RSS 2026 接收；2025.09 入学 SII（含中文点缀「开启人形机器人新篇章」）。保留 2-4 条，倒序。

### 3.3 Research（已批准）
双卡片网格：
- **NOW · 2025—**（赤陶描边卡）：Humanoid Whole-Body Control via RL；关键词丸 RL / WBC / Multi-Contact / Sim-to-Real
- **FOUNDATION · 2021—2025**（灰标卡）：Dexterous Hands & Robot Design；关键词丸 Mechanism Design / Underactuated Grippers / In-hand Manipulation / Force Sensing

### 3.4 Selected Publications（已批准）
6 篇，左图（250×150 灰底占位/真图）右文布局，条目间细分隔线。排序：

1. **RSS 2026** — Active Surface-Driven Reconfigurable Gripper (Zheng, **Zhu**, Wu, Wang, Dong)
2. **IEEE/ASME T-Mech 2025**（期刊绿徽）— An In-hand Gripper with Mecanum Wheel (**Zhu**, Zheng, Yang, Li, Dong)
3. **IJRR 2025**（期刊绿徽）— Enabling to Learn for Force Sensing (Dong, Li, **Zhu**, Guo, Lu)
4. **RSS 2024** — Multiple-DOF Underactuated Gripper with Force-Sensing via Deep Learning (Li, **Zhu**, Lu, Chen, Dong)
5. **IROS 2024** — Multiple-locomotion Origami Robot (**Zhu**, Guo, Yu, Sirag, Li, Dong, Dong)
6. **ReMAR 2024** — Stiffness of 3-PRS PM (Nigatu, Li, **Zhu**, Zhang, Guo, Lu, Kim)

每条：场馆徽章 + 年份 · 场馆全称 / 标题（衬线 20px）/ 作者行（Keqi Zhu 加粗）/ 一句话 TL;DR（斜体，依据检索到的论文摘要校准）/ 链接行（PAPER 必有，ARXIV / VIDEO / IEEE XPLORE 视检索结果）。

**论文图获取策略**（用户批准「我搜索+缺的你补」）：
- 检索顺序：arXiv 页 → 项目页 → IEEE/期刊公开预览图 → Google Scholar 关联页
- 找到公开 teaser/首图 → 下载至 `assets/pubs/<slug>.jpg`，压缩到 ≤200KB
- 找不到 → 统一风格占位图（米白底 + 赤陶描边 + 论文场馆缩写字样），并在交付说明中列出待补清单
- 版权处理：仅用作者自己论文的图（作者本人使用自己论文图属合理使用）；不盗用他人论文图

### 3.5 Education（已批准，重点板块）
赤陶节点竖时间线，3 项（SII Ph.D. 当前实心节点 / ZJU 硕士 / ZJU 竺院本科），每项：日期字距行 / 英文标题 / 中文小字（含 GPA 4.59/5 · 专业前 5）/ 一句话描述（关键词加粗）。

### 3.6 Industry + Honors（已批准，弱化双栏）
- 左：Suishi Robotics（上海燧氏）Lead Mechanical Designer，五版人形双臂机器人，公司被收购；字号小一档
- 右：Selected Honors 五条破折号列表（国奖 / 十佳大学生 / 省优毕 / 精雕杯全国银奖 / 数学竞赛国一）

### 3.7 Misc（已批准，默认折叠 `<details>`）
- 🧠 The Brain 最强大脑 8/9/10 季选手
- 🎓 Campus & Community（兼职辅导员/班长、ASES、晨兴文化中国人才计划）

### 3.8 Footer（已批准）
深炭黑底：左侧姓名 + © 2026 Keqi Zhu · Built with plain HTML/CSS · Hosted on GitHub Pages；右侧 SCHOLAR / GITHUB / EMAIL 链接。

## 4. 对外链接与隐私

- Google Scholar: `https://scholar.google.com/citations?user=6d3LDPgAAAAJ`
- GitHub: `https://github.com/SII-k7`
- Email: `laozhu0626@gmail.com`（mailto）
- **不出现**：手机号、QQ 邮箱、籍贯、政治面貌、年龄等隐私信息；不放 CV 下载

## 5. 文件结构

```
SII-k7.github.io/          （本地目录，用户自行推送）
├── index.html             单页全部内容
├── css/style.css          全部样式（自定义属性存色板）
├── assets/
│   ├── photo.jpg          个人照片（裁切/压缩后）
│   └── pubs/*.jpg         论文配图（搜到的真图 + 占位图）
├── favicon.svg            简单字标 KZ（赤陶色）
└── README.md              仓库说明 + 维护指南（如何加论文/换图）
```

## 6. 技术要求

- 语义化 HTML5（`<nav> <section> <article> <footer>`），锚点 `id` 对应导航
- CSS 自定义属性管理色板与字体栈；单文件 style.css
- 响应式：≥1024px 桌面双栏 Hero；768px 以下 Hero 竖排、论文图上文下、Research 单栏、双栏区单栏
- `scroll-behavior: smooth`；`<details>` 原生折叠（零 JS 也可用）
- 图片 `loading="lazy"`（首屏照片除外）、明确 width/height 防布局跳动
- SEO/分享：`<title>Keqi Zhu 朱科祺 — Robotics & Embodied AI</title>`、meta description、Open Graph 标签（og:image 用个人照片）
- 无外部字体/CDN 依赖（系统字体栈），保证十年可用

## 7. 验收标准

1. 本地直接打开 `index.html` 与设计稿三节视觉一致（暖调编辑风、全宽 1180px 布局）
2. 6 篇论文全部呈现，配图能搜到的用真图，缺失的用统一占位图并附待补清单
3. 手机宽度（≤480px）浏览无横向滚动、无重叠
4. 隐私信息零泄漏（检查 HTML 源码无手机号/QQ 邮箱等）
5. 所有对外链接可点击且正确
6. README 含维护指南与 GitHub Pages 发布步骤（供用户推送仓库用）

## 8. 实现阶段的自由裁量

TL;DR 文案、News 条目措辞、占位图样式细节、图片裁切构图由实现者依据检索结果微调，不改变已批准的布局与视觉系统。
