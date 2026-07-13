# GitHub Repository 設計

## Repository 分工表

| Repository | 用途 | 權限 |
| --------------- | ------------ | ------- |
| sungshu.github.io | 正式網站 | Public |
| blog | Hugo Playground / 測試區 | Public |
| blog-source | Hugo Source | Private |
| blog-docs | 完整文件 | Private |
| blog-ai-context | AI Context | Public |

---

## 各 Repository 說明

### sungshu.github.io

- 角色：Hugo Build 輸出目標
- 內容：網站靜態檔案（HTML、CSS、JS）
- 來源：由 GitHub Actions 自動 push，不手動編輯
- 訪客可透過 `https://sungshu.github.io` 瀏覽

### blog

- 角色：**Hugo Playground / 測試區**
- 用途：
  - 在進行關鍵設定變更前在此驗證
  - Theme 升版相容性測試
  - 新功能模板實驗
- **不保存正式內容**
- **不作為正式部署來源**
- 這個 Repo 的內容隨時可以丟棄或重建

### blog-source

- 角色：**所有正式內容的唯一來源**
- 內容：Hugo 專案（`content/`、`layouts/`、`themes/`、`static/admin/`）
- 權限設為 Private：保護未發佈草稿與設定不外洩

### blog-docs

- 角色：完整技術文件（Deploy SOP、圖片資源、德文債等）
- 目前狀態：Repo 已建立，内容待建立
- 預定 Phase 1 完成後開始填入

### blog-ai-context

- 角色：AI 協作上下文与專案交接文件
- 權限設為 Public：任何 AI 議將可直接取得上下文
- 需要隨欏架構決策同步更新

---

## 重要原則

- 正式文章必須優先寫入 `blog-source`，不要直接寫 `blog`
- `blog` 內容隨時可清除，不危岩網站穩定性
- `blog-ai-context` 是此專案的「對外開放直播間，讓 AI 快速接手的主要工具

---

## 更新歷史

| 版本 | 日期 | 變更 |
|---|---|---|
| v1.0 | 2026-07 | 初始建立 |
| v1.1 | 2026-07-13 | 補充 `blog` Repo 角色定位與各 Repo 詳細說明 |
