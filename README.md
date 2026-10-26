# Quiet Brick for Typora

一个为长时间阅读设计的 Typora 主题：中性灰正文、低饱和局部彩色，自动跟随系统深浅色。

正文使用苹方，标题用砖红色区分层级，代码使用紫灰、苔绿与陶土色，`==高亮==` 使用灰玫底色。除这些局部区域外，界面保持灰阶，避免整篇笔记出现明显色偏。

## 预览

仓库内提供两份自包含预览页，直接用浏览器打开即可：

- `preview/dark.html`：深色版
- `preview/light.html`：浅色版

预览页是用于检查配色和排版的浏览器模拟页面，不是 Typora 截图；页面里的数学源码编辑区、搜索命中、菜单和输入框是为了检查这些动态状态而模拟的。

## 特性

- 跟随系统深浅色自动切换，也可在 Typora 中为深浅模式分别选择本主题。
- 正文使用苹方与中性灰，长文阅读时没有明显色偏。
- 标题、代码语法和高亮标记使用低饱和局部彩色，信息层级清楚。
- 覆盖数学/HTML 源码编辑面板、搜索命中及其嵌套文字、代码语言浮层、目录提示、右键菜单、弹窗、输入框等动态状态，避免深色模式下出现默认白底或浅色字。
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

## 安装

1. 打开 Typora 的主题文件夹：
   - macOS：`~/Library/Application Support/abnerworks.Typora/themes/`
   - Windows：`%APPDATA%\Typora\themes\`
   - Linux：`~/.config/Typora/themes/`
2. 把 `quiet-brick.css` 复制进去。
3. 重启 Typora，或切换到其他主题再切回来。
4. 在「主题」菜单中选择 **Quiet Brick**。

如果分别设置了浅色与深色主题，两处都选择 Quiet Brick 即可。

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
| `--border-color` | 边框与分隔线 |
| `--selection-bg-color` | 选区与搜索命中背景 |
| `--mark-bg-color` | `==高亮==` 背景 |
| `--syntax-*` | 代码语法色（注释、关键字、字符串、数字等） |

整体字号由 `html { font-size }` 控制，也可以在 Typora 偏好设置中调整。正文行距、最大宽度分别在 `#write` 的 `line-height` 与 `max-width` 中修改。

## 性能

- 单文件约 13.6 KB，89 条规则。
- 无动画、过渡、滤镜、阴影、`will-change` 或 `!important`。
- 无网络字体、图片与脚本，不产生额外网络请求或持续渲染开销。
- 只覆盖必要的排版与控件颜色，尽量保留 Typora 默认行为。

## 已知限制

- Mermaid、流程图等图表有自身的主题配色，本主题没有强制覆盖，以免深色背景配上深色线条。
- 导出 PDF 或打印没有单独适配，输出效果取决于 Typora 的默认导出样式。
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

当前版本：v1.0.0（2026-10-26）

## 许可证

[MIT](LICENSE)
