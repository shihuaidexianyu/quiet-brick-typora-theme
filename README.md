# Quiet Brick for Typora

一个为长时间阅读设计的 Typora 主题：中性灰正文、低饱和局部彩色，屏幕上自动跟随系统深浅色，打印与导出 PDF 时始终输出白纸浅色版。

正文使用苹方，标题用砖红色区分层级，代码使用紫灰、苔绿与陶土色，`==高亮==` 使用灰玫底色。除这些局部区域外，界面保持灰阶，避免整篇笔记出现明显色偏。

## 预览

仓库内提供两份自包含预览页，直接用浏览器打开即可：

- `preview/dark.html`：深色版
- `preview/light.html`：浅色版

预览页是用于检查配色和排版的浏览器模拟页面，不是 Typora 截图；页面里的数学源码编辑区、搜索命中、提示块、脚注、菜单和输入框是为了检查这些状态而模拟的。在浏览器中打印任一预览页（即使是深色版），可以直接检查打印配色。

## 特性

- 跟随系统深浅色自动切换，也可在 Typora 中为深浅模式分别选择本主题。
- 正文使用苹方与中性灰，长文阅读时没有明显色偏。
- 标题、代码语法和高亮标记使用低饱和局部彩色，信息层级清楚。
- 正文、次要文字、行号、语法高亮、提示块标题在浅色、深色和打印三套配色下均达到 WCAG AA（≥ 4.5:1）。
- 覆盖数学/HTML 源码编辑面板、搜索命中（含 Typora 1.10+ 的 `::highlight` 搜索高亮）、代码语言浮层、目录提示、右键菜单、弹窗、输入框等动态状态，避免深色模式下出现默认白底或浅色字。
- 适配 GitHub 风格提示块（`> [!NOTE]` 等）、`[TOC]` 目录、脚注、专注模式和 Mermaid 图表的深浅色。
- 专门的打印 / PDF 样式，见下文「打印与导出 PDF」。
- 单文件 CSS，无外部字体、图片、脚本或网络请求。

## 依赖说明

### 运行环境

| 项目 | 要求 |
| --- | --- |
| Typora | 建议 1.0 及以上；主题使用 CSS 自定义属性与 `prefers-color-scheme`，更早版本未验证 |
| 操作系统 | macOS、Windows、Linux |
| 构建工具 | 无。不需要 npm、Node、Python 或任何打包步骤 |
| 网络 | 安装和使用都不需要联网 |

### 字体

主题不附带字体文件，也不下载网络字体，全部使用系统已有字体：

| 用途 | 首选 | 回退 |
| --- | --- | --- |
| 正文、标题 | 苹方 PingFang SC（macOS） | 微软雅黑（Windows）、Noto Sans CJK SC（Linux）、系统无衬线字体 |
| 代码、行号 | SF Mono / Menlo（macOS） | Consolas（Windows）、Liberation Mono（Linux）、系统等宽字体 |

如果系统缺少上述字体，会自动使用通用回退字体，不影响功能，只是字形会有所不同。

### 预览页

预览页是纯 HTML/CSS，没有 JavaScript 依赖，用任意现代浏览器打开即可。

## 安装与更新

### 方式一：直接复制

1. 打开 Typora 的主题文件夹：
   - macOS：`~/Library/Application Support/abnerworks.Typora/themes/`
   - Windows：`%APPDATA%\Typora\themes\`
   - Linux：`~/.config/Typora/themes/`
2. 把 `quiet-brick.css` 复制进去。
3. 重启 Typora，或切换到其他主题再切回 **Quiet Brick**。

> 注意：每次拉取或修改本仓库后，都需要重新把最新的 `quiet-brick.css` 覆盖到 Typora 主题目录并重选一次主题，修改才会在 Typora 中生效。

### 方式二：软链接（macOS / Linux 推荐）

如果经常拉取更新或自行调整配色，可以直接把仓库里的 `quiet-brick.css` 软链接到 Typora 主题目录，后续更新仓库无需手动重复复制：

```zsh
# macOS
ln -sf "$(pwd)/quiet-brick.css" ~/Library/Application\ Support/abnerworks.Typora/themes/quiet-brick.css
```

如果分别设置了浅色与深色主题，两处都选择 **Quiet Brick** 即可。

## 打印与导出 PDF

| 问题 | 处理方式 |
| --- | --- |
| 深色模式下导出 PDF 出现黑底或四周大黑边 | macOS 版 Typora 在深色模式下导出 PDF 时，一方面会在 `<head>` 末尾注入 `#typora-wkprint-style`（`@page { margin: 72pt !important }`），使 WebKit 将页面绘制裁剪在 `72pt` 页边距框内、四周留出透明区；另一方面底层 `TyPDFEngine` 会先用当前窗口的深色背景（`#1c1c1c`）填满整张 A4 画布，再把透明边距的页面叠画上去，从而露出四周大黑框。主题在 `@media print` 中使用更高优先级的 `@page :first / :left / :right { margin: 0 !important }` 接管裁剪框，用全页纯白底色覆盖整张纸面（含末页底部留白区），再由 `#write` 与块级内边距控制四边页边距与跨页上下留白，彻底消除黑边 |
| 页面灰底、浅灰细线在纸上发虚 | 打印时背景改为纯白 `#ffffff`，正文加深到 `#222222`，表格线、引用线、分隔线全部加深 |
| 长代码行被纸张边缘截断 | 代码块与 YAML 强制自动换行（`pre-wrap`），即使关闭了「代码块自动换行」也不会被裁切 |
| 高亮、表头、代码块底色在系统打印对话框中丢失 | 对承载语义的底色元素设置 `print-color-adjust: exact` |
| 标题落在页尾、引用或图片被拆到两页 | 标题后禁止分页（`break-after: avoid-page`）；引用、提示块、图片、公式、图表、表格行、列表项禁止内部分页（`break-inside: avoid`） |
| 16.5px 正文在 A4 上偏大 | 打印正文缩至 14px（约五号字 10.5pt），行距由 1.85 收紧至 1.7 |

### 打印页边距说明

- 纸张大小、页眉页脚请在 Typora「偏好设置 → 导出 → PDF」中设置。
- 为阻止 WebKit `@page` 外边距变成透明区并透出深色底，导出 PDF 的页面四周留白由 `quiet-brick.css` 的 `@media print` 统一接管（默认上下 `46pt`、左右 `50pt`，跨页断点自动保留上下白边），无需在 Typora 导出设置里额外加大页边距。

## 自定义

主题的颜色集中在文件顶部的 `:root` 与 `@media (prefers-color-scheme: dark)` 两个变量块中，修改后无需构建。

常用变量：

| 变量 | 作用 |
| --- | --- |
| `--bg-color` | 页面背景 |
| `--text-color` | 正文颜色 |
| `--title-color` | 标题颜色 |
| `--surface-color` | 代码块、浮层、编辑面板背景 |
| `--surface-raised-color` | 行内代码、输入框、表头背景 |
| `--border-color` | 代码块、行内代码等元素的细边框 |
| `--rule-color` | 引用竖线、提示块竖线、分隔线等结构线 |
| `--table-border-color` | 表格线 |
| `--selection-bg-color` | 选区背景 |
| `--search-hit-bg-color` / `--search-select-bg-color` | 搜索命中 / 当前命中背景 |
| `--mark-bg-color` | `==高亮==` 背景 |
| `--syntax-*` | 代码语法色（注释、关键字、字符串、数字等） |
| `--alert-*` | 提示块颜色（note、tip、important、warning、caution） |

打印配色与打印页边距在文件末尾的 `body.typora-print` 与 `@media print` 中单独定义，修改屏幕配色不会影响打印效果。如需调整导出 PDF 的页边距，可修改 `@media print` 中 `#write` 的 `padding: 46pt 50pt !important`。

整体字号由 `html { font-size }` 控制，也可以在 Typora 偏好设置中调整。正文行距、最大宽度分别在 `#write` 的 `line-height` 与 `max-width` 中修改。

## 性能

- 单文件约 24 KB，日常屏幕样式无 `!important`、动画、过渡、滤镜、阴影或 `will-change`（仅在 `@media print` 中使用必要的 `!important` 与纯白底色延展，用于覆盖 Typora 导出 PDF 时动态注入的 `#typora-wkprint-style` 与 `#style-mac-print`）。
- 无网络字体、图片与脚本，不产生额外网络请求或持续渲染开销。
- 只覆盖必要的排版与控件颜色，尽量保留 Typora 默认行为。

## 已知限制

- Mermaid 通过 Typora 的 `--mermaid-theme` 变量在浅色下使用 `neutral`、深色下使用 `dark`；切换系统深浅色后需重新打开文档才会重绘图表。若在深色模式下导出含 Mermaid 的 PDF，图表可能保留深色样式，建议导出前切到浅色模式。flowchart.js / 时序图（`flow`、`sequence`）没有主题变量，深色模式下线条对比度仍取决于 Typora 默认样式。
- Typora 不同版本的 DOM 可能变化，个别动态控件（例如较新的浮层、菜单）可能需要随版本补充选择器。
- 主要在 macOS 上开发和检查；Windows 与 Linux 未做实机验证。
- 主题只做样式，不改变 Typora 的功能或快捷键。

## 目录结构

```text
quiet-brick-typora-theme/
├── quiet-brick.css          # 主题本体，复制到 Typora 主题文件夹
├── preview/
│   ├── dark.html            # 深色预览
│   └── light.html           # 浅色预览
├── README.md
├── LICENSE
└── .gitignore
```

## 版本

当前版本：v1.1.1（2026-11-15）

- v1.1.1（2026-11-15）：彻底修复 macOS 版 Typora 在深色模式下导出 PDF 时的四周 `#1c1c1c` 黑边问题（通过 `@page :first / :left / :right` 覆盖 `#typora-wkprint-style` 页边距裁剪框，配合全页纯白底色及多页跨页留白方案）。
- v1.1.0（2026-11-14）：新增打印 / PDF 样式；修正浅色次要文字、行号对比度不足；加强细线与选区可见度；修正深色模式下脚注角标、高亮标记、专注模式失效；搜索当前命中原为浅灰底白字，改为砖红底；新增提示块、目录、Mermaid 适配。
- v1.0.0（2026-10-26）：首个版本。

## 许可证

[MIT](LICENSE)
