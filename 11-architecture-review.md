# 11 - 架構審查報告

> 審查日期：2026-07-13
> 審查者：Comet AI（Perplexity）
> 基於文件版本：v1.0

---

## 一、完整架構整理

整個專案由 5 個 GitHub Repository 組成，分工如下：

| Repository | 角色 | 權限 |
|---|---|---|
| `sungshu.github.io` | 靜態網站輸出，訪客瀏覽 | Public |
| `blog` | 測試沙盒 | Public |
| `blog-source` | Hugo Source + Markdown 文章 | Private |
| `blog-docs` | 完整技術文件 | Private |
| `blog-ai-context` | AI 交接與協作上下文 | Public |

資料流向：

```
瀏覽器訪客
  └─> sungshu.github.io（Public）
        └─ Hugo Build
              └─ blog-source（Private）
                    └─ Decap CMS
                          └─ GitHub OAuth
```

---

## 二、已確認決策

- **技術棧**：Hugo + Stack Theme + Decap CMS，不回 Blogger，Source 保持 Private
- **Repository 分離架構**：5 Repo 分工，Public/Private 已定
- **CMS 認證方式**：透過 `sungshu.github.io/admin` + GitHub OAuth 連接 Private `blog-source`
- **Hugo 目錄結構**：`blog-source` 含 `content/posts`、`themes/stack`、`admin/`（Decap）
- **Theme 客製化**：已完成 CSS 調整、移除文章 Card、隱藏 Disqus 空白區
- **開發分階段（Phase 1~4）**：Phase 1 為基礎建設，Phase 4 為 GA4/Ads 營運

---

## 三、尚未決定事項

| 項目 | 狀態 | 說明 |
|---|---|---|
| Waline 部署位置 | 未定 | Phase 3 列入，但未說明是自架（VPS）還是 Waline Cloud |
| View Count / Like 方案 | 未定 | 參考了 Umami 與 Waline，尚未選定 |
| Search 方案 | 未定 | Phase 3 列入，Fuse.js 或 Algolia 尚未確認 |
| GA4 / GSC 設定細節 | 未定 | Phase 4 提及，無 Property ID 記錄 |
| Ads / Reward 方式 | 未定 | Phase 4 列入，完全未規劃 |
| `blog-docs` 內容 | 空白 | Repo 已建立但內容為空 |
| Theme 深度客製化方向 | 未定 | 只記錄已做的，未來改動尚未規劃 |

---

## 四、文件間潛在衝突

### ⚠️ 衝突點 1：Source 目錄結構 vs URL 設計

- `04-hugo-structure.md`：`content/posts` 下無月份層
- `06-content-url-design.md`：URL 設計包含月份 `/posts/2026/07/hugo-blog/`
- **問題**：若 `hugo.toml` 未設定 `[permalinks]`，URL 會少月份，與設計不符
- **建議**：確認 `hugo.toml` 已加入以下設定：

```toml
[permalinks]
  posts = "/posts/:year/:month/:slug/"
```

### ⚠️ 衝突點 2：Decap CMS `/admin` 部署流程未說明

- `02-architecture.md` 與 `04-hugo-structure.md` 確認 `admin/` 在 `blog-source`
- 但 `admin/` 要透過 `sungshu.github.io/admin` 對外提供
- **問題**：需確認 Hugo Build workflow 有正確 copy `static/admin/` 到 Public Repo
- **建議**：在 `05-cms-design.md` 補充 GitHub Actions 步驟說明

### ⚠️ 衝突點 3：`blog` Repo 角色模糊

- `03-github-repository-design.md` 說 `blog` 是「測試區」且為 Public
- 後續所有文件都以 `blog-source` 為主軸，`blog` 完全沒有再出現
- **建議**：明確定義其用途或標記為 archived

---

## 五、架構 Review（針對 6 個面向）

### 1. GitHub Repository 分離是否合理？

**結論：合理，設計得當。**

Public/Private 分離讓文章內容、草稿、設定不外洩，同時保持網站公開瀏覽。對個人長期經營的技術 Blog 而言，這是最穩健的結構。

**建議**：釐清 `blog` 的定位，在 `03-github-repository-design.md` 補充說明，避免日後混淆。

### 2. Decap CMS 接 Private Repository 是否可行？

**結論：技術上完全可行。**

Decap CMS 支援 GitHub OAuth backend，透過 token 存取 Private Repo 是標準做法。

**建議**：在 `05-cms-design.md` 補充以下內容：
- GitHub OAuth App 設定步驟
- Callback URL 設定
- 所需 token scope（`repo` 權限）
- GitHub Actions workflow 需要的 `GITHUB_TOKEN` 權限設定

### 3. 是否有安全問題？

**結論：整體良好，有一個盲點需注意。**

`sungshu.github.io/admin` 是公開可訪問的 URL，任何人都能開啟 CMS 登入頁。雖然實際寫入需要 GitHub OAuth 認證，但建議加第二道防線。

**建議**：
- 目前：確保 OAuth App 只允許 `sungshu` 帳號登入
- 未來（若遷移至 VPS/Nginx）：可加 HTTP Basic Auth 或 IP 限制
- 在文件中補充此安全考量說明

### 4. 是否有維護問題？

**結論：輕微管理負擔，主要風險在文件同步。**

- **`blog-ai-context` 需要同步更新**：每次架構決策後需更新，否則 AI Context 會與實際狀態脫節
- **GitHub Actions Workflow 是維護核心**：Hugo 升版或 Stack Theme 更新時需測試相容性
- **`blog` Repo 角色不清**：造成潛在的維護混淆

**建議**：建立 CHANGELOG 更新規範，每次架構決策後同步更新 `blog-ai-context`。

### 5. 未來文章數量增加後是否可擴展？

**結論：可以擴展，有一個潛在瓶頸需預先了解。**

- `content/posts/年/` 按年分資料夾，5~10 年完全夠用
- Hugo 靜態輸出在文章數達 1000+ 篇時，Build 時間會明顯增長
- 目前階段無需擔心

**建議**：Phase 2~3 時研究 Hugo 的 `--minify` 與 incremental build 選項。

### 6. 是否適合長期經營 5~10 年？

**結論：適合，選型保守穩健。**

Hugo 是成熟的靜態產生器，Stack Theme 持續維護，GitHub Pages 免費且穩定。

**主要風險點**：

| 風險 | 說明 | 緩解方式 |
|---|---|---|
| Decap CMS 社群活躍度 | 相對小眾，若停止維護需遷移 | 內容是純 Markdown，遷移成本極低 |
| Stack Theme 升版相容性 | 客製化 CSS 可能受影響 | 在 `07-theme-customization.md` 記錄每次改了哪個原始檔 |
| GitHub Pages 服務變動 | 極低機率但需預案 | 靜態檔案可快速遷移至 Cloudflare Pages 或 Netlify |

---

## 六、下一階段開發建議（優先順序）

| 優先 | 項目 | 說明 |
|---|---|---|
| P1 | 確認 `hugo.toml` permalink 設定 | 修復 URL 月份不一致問題 |
| P1 | 補充 `05-cms-design.md` | 加入 OAuth App 設定步驟與 token scope |
| P2 | 釐清 `blog` Repo 用途 | 更新 `03-github-repository-design.md` |
| P2 | 建立 `blog-docs` 內容 | Phase 1 完成後開始記錄 Deploy SOP |
| P3 | 建立 CHANGELOG 更新規範 | 保持 AI Context 與實際狀態一致 |
| P3 | 記錄 Theme 客製化改動檔案 | 方便日後 Stack Theme 升版時 diff |

---

## 附記

- 本文件由 Comet AI 基於 `blog-ai-context` v1.0 所有 Markdown 文件審查產出
- 本審查**不推翻現有設計**，以現有架構為基礎提出改善
- 下次 AI 協作時可直接引用本文件作為架構現狀參考
