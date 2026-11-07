# 工艺智能问答 RAG 系统 · 前端演示 Demo

纯前端、单页、可离线运行的交互演示。无后端、无数据库、无构建步骤。

**在线地址**：https://Hhhhu3494.github.io/process-demo/

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

## 本地预览

```bash
cd dist
python -m http.server 8080
# 打开 http://localhost:8080
```

直接双击 `index.html` 用 `file://` 打开通常也能工作，但建议走本地 HTTP，避免浏览器对本地文件的限制。

## 部署状态

本仓库 `Hhhhu3494/process-demo` 已启用 GitHub Pages：

| 项 | 值 |
|---|---|
| 发布源 | `main` 分支 / `/(root)` |
| 站点地址 | `https://Hhhhu3494.github.io/process-demo/` |
| 站点大小 | 约 2.2 MB，纯静态，无需构建 |
| 外部请求 | 零，依赖全部在 `vendor/` |

`.nojekyll` 保证 `vendor/` 不会被 Jekyll 过滤；页面资源全为相对路径，子路径部署可正常加载。

## 后续新增 Demo

本仓库拟容纳多个前端演示。新增时请放在**独立子目录**，Pages 会按子路径提供，例如：

```
https://Hhhhu3494.github.io/process-demo/contract-demo/
```

当前 RAG Demo 位于仓库根目录，地址不带子路径。`.nojekyll` 保留在仓库根目录即可，子目录无需重复放置。

## 更新内容

本地工作区根目录的 `index.html` 是**源文件**（CDN 版，便于本地开发）；本仓库根目录的 `index.html` 是**发布副本**（依赖本地化）。两者不可混用：改动源文件后必须重新生成发布副本，否则线上不会变化。

维护者使用本地工作区的 `sync-github.mjs`（不随本仓库分发）完成同步：重新构建 → 离线包自检 → 比对远端差异 → 单个 commit 推送 → 站点验证。脚本支持 `--dry-run` 只自检不推送；推送凭据通过环境变量临时提供，**不写入仓库里的任何文件**，用完立即在 Developer settings → Tokens 中撤销。

> 平台一键发布的线上链接是覆盖式更新，改完本地文件不会自动同步，需要显式重新发布。

## 依赖与许可

Vue 3.5.45 / Element Plus 2.7.0 / Element Plus Icons Vue 2.3.1 随包分发，均为各自开源许可。对外分发前请按项目实际合规要求核对许可与署名。

> Pages 站点即使仓库设为私有也会公开到互联网，别放敏感数据。
