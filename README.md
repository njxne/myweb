# aicode — 终端 & AI（GitHub Pages 版）

纯静态网页终端：用命令操作一个浏览器本地文件系统，并能和 AI 对话。
**无需任何后端**，直接托管在 GitHub Pages 上即可使用。

> 与本地版（`virtual-terminal/`，带 `server.js` 读写真实文件）不同：
> 本版运行在静态托管环境，没有后端，所以文件保存在**浏览器本地**（localStorage）。
> 两者互不影响，各自独立。

## 部署到 GitHub Pages

1. 新建一个 GitHub 仓库，把本文件夹里的所有文件推上去：

   ```bash
   git init
   git add .
   git commit -m "aicode-shell pages"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```

2. 打开仓库 **Settings → Pages**：
   - **Source** 选 `GitHub Actions`（仓库里已带 `.github/workflows/pages.yml`，推送后自动部署）；
   - 或选 `Deploy from a branch` → 分支 `main`、目录 `/ (root)`。
3. 稍等片刻，访问 `https://<你的用户名>.github.io/<仓库名>/` 即可。

> 仓库里带了 `.nojekyll`，避免 GitHub 的 Jekyll 处理影响静态资源。

## 打开页面后

- **文件命令**：`ls` `cd` `pwd` `cat` `echo > 文件` `mkdir` `touch` `rm [-r]` `cp` `mv` `tree` `find` `stat`
- **文本处理**：`grep` `head` `tail` `wc` `rev` `seq` `base64`
- **AI 对话**：输入 `ai` 进入对话模式，直接打字发送，`exit` 退出
- **其他**：`help`（命令 + 中文说明）`clear` `theme` `sysinfo` `neofetch` `calc` `banner` `cowsay` `fortune` `history` `settings`

示例：

```bash
mkdir notes
echo hello > notes/a.txt
cat notes/a.txt
tree
```

## 配置 AI

点右上角 **⚙**，填写：

- **Base URL**：接口地址，如 `https://api.openai.com`
- **API Key**：`sk-...`
- 点 **拉取模型**，下拉里选一个模型 → **保存**

接口需兼容 OpenAI（`/v1/models` 拉取模型、`/v1/chat/completions` 对话）。
配置与文件都存在浏览器本地，AI 只用于对话，**不会读取或修改你的文件**。

> 注意：部分 AI 接口对跨域（CORS）有限制。若浏览器控制台报 CORS 错误，说明该接口不允许网页直接调用，需要改用支持 CORS 的接口或自建代理。

## 安全提示（重要）

- 本项目代码里**不含任何 API Key**。Key 由你在页面「设置」里手动填写，只保存在**你自己的浏览器**（localStorage）中，不会写进任何文件，也不会随页面发布。
- **不要把 Key 写进代码、README 或任何会被提交的文件**，否则推到 GitHub 后人人可见。仓库已带 `.gitignore`（忽略 `.env`、`*.key` 等），但请仍自行留意。
- 页面是在你的浏览器里直接用 Key 调用 AI 接口的（BYOK）。请使用支持跨域、且允许浏览器调用的接口；若担心 Key 暴露，建议使用受限权限的 Key，或自建一个只在本地运行的中转服务。
- 在公共电脑上用完，请在「设置」里点 **清空 Key**，或使用无痕模式。

## 数据说明

- 文件保存在浏览器的 `localStorage` 中，**仅当前浏览器可见**，换设备 / 换浏览器 / 清缓存会丢失。
- 主题、字号、命令历史、AI 配置同样保存在本地。

## 界面

- **竖屏**：全屏终端 + 底部输入框，手机键盘弹出时输入框不会被遮挡。
- **横屏**：左侧一栏显示 AICODE 标识和环境信息（当前路径、条目数、主题、AI 状态），右侧是终端。

## 文件结构

```
.
├── index.html                        # 前端页面（终端 + AI，单文件）
├── README.md
├── .nojekyll
├── .gitignore                        # 忽略 .env / *.key 等密钥文件
└── .github/workflows/pages.yml       # GitHub Actions 自动部署
```
