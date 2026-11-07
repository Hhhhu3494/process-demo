# 工艺智能问答 RAG 系统 · 前端演示 Demo

纯前端、单页、可离线运行的交互演示。无后端、无数据库、无构建步骤。

## 目录结构

```
.
├── index.html          # 入口页（全部逻辑内联）
├── vendor/             # 本地化依赖，页面不请求任何外部 CDN
│   ├── vue.global.js                    Vue 3.5.45
│   ├── element-plus.css                 Element Plus 2.7.0 样式
│   ├── element-plus.full.min.js         Element Plus 2.7.0 完整 UMD
│   └── element-plus-icons.iife.min.js   Element Plus Icons Vue 2.3.1
├── .nojekyll           # 让 GitHub Pages 原样提供 vendor/ 目录
└── README.md
```

依赖均为各自开源许可，随包分发前请按项目实际合规要求核对许可与署名。

## 本地预览

```bash
cd dist
python -m http.server 8080
# 打开 http://localhost:8080
```

直接双击 `index.html` 用 `file://` 打开通常也能工作，但建议走本地 HTTP，避免浏览器对本地文件的限制。

## 发布到 GitHub Pages —— 四步完整指引

约 5 分钟：建公开仓库 → 生成推送令牌 → 本地推送 → 打开 Pages。

### 第 1 步 · 建一个公开仓库

登录后右上角 **+** → **New repository**：

| 字段 | 填法 |
|---|---|
| Repository name | `process-rag-demo`；若命名为 `<用户名>.github.io`，访问地址是根路径 |
| Public / Private | 必须选 **Public**（免费账号的 Pages 只对公开仓库开放） |
| Add a README / .gitignore / license | **三项都不要勾** |

最后一项很关键：本目录已有文件和 `.nojekyll`，仓库若自带 README 会让首次 push 冲突。点 **Create repository** 即可。

### 第 2 步 · 生成推送授权（PAT）

**网页登录 ≠ 命令行能推**。Git 推送需要单独凭据；本机没有凭据管理器，用 Personal Access Token 最省事。

头像 → **Settings** → 左侧 **Developer settings** → **Personal access tokens** → **Tokens (classic)** → **Generate new token** → **Generate new token (classic)**：

- **Note**：`rag-demo-deploy`
- **Expiration**：`7 days`（一次性任务，用完即删）
- **Select scopes**：勾 `repo`（命令行推送必需）
- 点 **Generate token** → **立刻复制，令牌只显示这一次**

> GitHub 官方更推荐 fine-grained token；但一次性推送用 classic + `repo` 步骤最少。令牌等同账号权限，不要写进任何会被提交的文件，也不要发给别人。

### 第 3 步 · 本地推送

在**本目录**打开 Git Bash：

```bash
git init
git config user.name  "你的 GitHub 用户名"
git config user.email "你 GitHub 账号已验证的邮箱"
git add .
git commit -m "Add offline RAG demo"
git branch -M main
git remote add origin https://github.com/<用户名>/process-rag-demo.git
git push -u origin main
```

推送时问 `Username` / `Password`：**Username 填用户名，Password 直接粘贴第 2 步的令牌**（不是账号密码）。令牌不会存到磁盘。

> 邮箱必须已在 GitHub 验证过，否则 Pages 可能不触发构建。

**若 push 卡住超时**：本机直连 `github.com` 不通，但 `api.github.com` 可直连。若浏览器是靠代理插件登上 GitHub 的，把该代理的本地端口配给 Git（常见 `7890` / `10809`，换成实际值）：

```bash
git config --global http.proxy  http://127.0.0.1:<端口>
git config --global https.proxy http://127.0.0.1:<端口>
# 推完可清除
git config --global --unset http.proxy
git config --global --unset https.proxy
```

仍不通时，可改用 REST API 上传（走 `api.github.com`），无需 git push。

> 不要用网页 "Add file → Upload files" 代替推送：`vendor/` 是子目录、`.nojekyll` 是隐藏文件，网页拖拽容易漏掉。

### 第 4 步 · 打开 Pages

仓库 → **Settings** → 侧栏 "Code, planning, and automation" 区的 **Pages** → Build and deployment → **Source** 选 `Deploy from a branch` → Branch 选 `main`、文件夹选 `/(root)` → **Save**。

等 1–2 分钟，页面顶部出现访问地址：

- 仓库名 `process-rag-demo` → `https://<用户名>.github.io/process-rag-demo/`
- 仓库名 `<用户名>.github.io` → `https://<用户名>.github.io/`

`.nojekyll` 保证 `vendor/` 不被 Jekyll 过滤；资源全为相对路径，子路径部署可正常加载。站点约 2.2 MB，无需构建步骤。

> Pages 站点即使仓库设为私有也会公开到互联网，别放敏感数据。

### 收尾 · 撤销令牌

头像 → Settings → Developer settings → Personal access tokens → Tokens (classic) → 该令牌右侧 **Delete**。以后要更新内容，重新生成一个短期令牌即可。

## 更新内容

根目录的 `index.html` 是**源文件**（仍用 CDN 引用，方便本地开发）。`dist/index.html` 是**发布副本**。改完源文件后：

```bash
node build-dist.mjs     # 从源文件重新生成 dist/index.html（自动替换为本地引用）
cd dist && git add . && git commit -m "Update demo" && git push
```

平台一键发布的线上链接是**覆盖式**更新，改完本地文件不会自动同步，需要显式重新发布。
