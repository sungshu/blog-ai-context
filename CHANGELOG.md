# Changelog

## v1.1 - 2026-07-13

架構審查與工程規格資料劃大升級（Engineering Specification 階段）

### 新增文件

- `11-architecture-review.md` — 完整架構審查報告（由 Comet AI 審查）
- `11-content-frontmatter.md` — 文章 Frontmatter 標準（slug、date、draft 規則）
- `12-hugo-config-reference.md` — Hugo permalink 設定與 URL 產生連結說明
- `13-cms-authentication.md` — Decap CMS OAuth 流程與 config.yml 標準
- `14-deployment-design.md` — GitHub Actions 完整 Workflow 與 DEPLOY_TOKEN 設定
- `15-content-workflow.md` — 文章撰寫與發佈流程（CMS 模式 / Git 模式）
- `16-maintenance-guide.md` — 長期維護指南（Theme 升版、Hugo 升版、風險緩解）

### 更新文件

- `03-github-repository-design.md` — 補充 `blog` Repo 角色定位與各 Repo 詳細說明

### 架構單页對照

| 問題 | 審查建議 | 處理方式 |
|---|---|---|
| Source 路徑 vs URL 月份不一致 | 必須設定 `[permalinks]` | `12` 記錄 |
| Decap CMS admin 流程不明 | OAuth 流程與 config 分離說明 | `13` 記錄 |
| `blog` Repo 角色模糊 | 定義為 Playground / 測試區 | `03` 更新 |
| 安全性：`/admin` 公開 | 目前僅 OAuth，未來可加 Cloudflare Access | `13` 記錄 |

---

## v1.0

建立 AI Context Repository

完成：

- GitHub 架構規劃
- Public / Private 分離
- Hugo 架構確認
