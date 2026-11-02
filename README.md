# 知行笔记

Hugo + PaperMod 静态博客。正式地址：https://ziyiic.github.io/

## 本地预览（当前电脑）

在本目录打开 PowerShell：

```powershell
& '..\.tools\hugo\hugo.exe' server
```

浏览器打开终端输出的地址，通常为 http://localhost:1313/ 。包含草稿时追加 `-D`，停止用 `Ctrl+C`。

Hugo Extended 0.169.0 安装在相邻的 `../.tools/hugo/`，未修改系统 PATH；下载已校验官方 SHA-256。该工具目录不在博客仓库中。

## 换电脑

安装 Hugo Extended 0.169.0 和 Git，然后：

```powershell
git clone --recurse-submodules https://github.com/ziyiic/ziyiic.github.io.git
cd ziyiic.github.io
hugo server
```

若已普通克隆，执行 `git submodule update --init --recursive` 下载主题。

## 写文章

```powershell
& '..\.tools\hugo\hugo.exe' new content posts/my-topic/index.md
```

编辑 `content/posts/my-topic/index.md`。文章顶部使用 YAML 元信息，发布前将 `draft` 改为 `false`，日期不要晚于当前时间。图片放在文章旁边，以 `![说明](图片名.png)` 引用。文件保存为 UTF-8。

`content/about.md` 为关于页，`hugo.yaml` 为网站配置。不要直接修改主题，定制样式放在 `assets/css/extended/`。

## 发布与验收

先运行正式构建，默认排除草稿与未来文章：

```powershell
& '..\.tools\hugo\hugo.exe' --gc --minify
```

只提交文章、配置、静态资源与主题子模块版本，不提交 `public/`、`resources/_gen/` 或密钥。仓库是公开的，源文件和提交历史都能被访问。

推送到 `main` 后，`.github/workflows/hugo.yaml` 构建并部署 GitHub Pages。仓库 Settings → Pages 的 Source 应为 GitHub Actions。工作流固定 Hugo 版本并校验下载校验和；升级时同步更新版本和校验和。

验收不只看 Actions 成功，还应打开线上首页、文章、分类与关于页，检查图片、代码高亮、主题切换及手机布局，并确认线上出现本次修改。

## 初始版本

- 中文首页、文章列表、分类与关于页。
- 首篇 STM32 复刻路线文章，含图片、表格和代码示例。
- 明暗主题、目录、代码复制与移动端排版。
- GitHub Actions 自动构建部署；无评论、统计、数据库或自定义域名。

主题来源：https://github.com/adityatelange/hugo-PaperMod
Hugo 文档：https://gohugo.io/
