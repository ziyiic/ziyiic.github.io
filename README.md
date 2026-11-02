# 知行笔记

Hugo + PaperMod 静态博客。正式地址：https://ziyiic.github.io/

## 本地预览（当前电脑）

本机项目目录为 `D:\Blog\UserCode\my-blog`，Hugo 工具保存在相邻的 `D:\Blog\UserCode\.tools\hugo`。项目和工具已一起从原会话目录迁移，线上地址不变。

打开 PowerShell 后执行：

```powershell
Set-Location 'D:\Blog\UserCode\my-blog'
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

### 代码外观

代码采用自托管的 JetBrains Mono（常规、粗体）与 Monokai 高亮，字体不依赖第三方 CDN。中文依次回退到本机的 Noto Sans Mono CJK SC、更纱黑体、微软雅黑或系统等宽字体；不同设备的中文外观可能不同。代码字号 15px、行高 1.8，关闭编程连字，中文注释取消斜体并提高对比度。明暗页面均保留 Monokai 深色代码块，手机长行在代码块内横向滚动。

字体与许可保存在 `static/fonts/jetbrains-mono/`，上游为 https://github.com/JetBrains/JetBrainsMono ，遵循 SIL Open Font License。字体、间距及注释调整见 `assets/css/extended/blog.css`。

`assets/css/includes/chroma-styles.css` 覆盖主题自带色表，使用 `hugo gen chromastyles --style=monokai` 生成；保持 `markup.highlight.noClasses: false`，无需修改主题子模块。

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

## 首次上线验收（2026-11-03）

- 正式构建成功，线上首页、文章、分类与关于页可访问。
- 浏览器实际操作验证：图片加载、代码高亮与复制、明暗主题切换正常。
- 1440px 桌面和 390px 手机视口已检查；390px 下页面宽度与视口相等，无整页横向溢出。此项是浏览器模拟，不是实体手机测试。
- 线上页面使用正式 canonical 地址，允许索引，无开发热刷新脚本，浏览器未记录错误。

当前电脑直连 GitHub 推送失败，但通过已存在的本地代理成功；如普通推送再次连接超时，可在代理运行时使用：

```powershell
git -c http.proxy=http://127.0.0.1:7899 push
```

该参数只作用于这次命令，不修改系统或全局 Git 设置。
