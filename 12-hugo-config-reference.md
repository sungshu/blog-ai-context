# 12 - Hugo 設定參考文件

> 版本：v1.1
> 建立日期：2026-07-13
> 狀態：已確認必須設定

---

## 一、Content 目錄結構 vs 輸出 URL 對照

這是目前最重要的技術確認點。Hugo 預設不會自動將月份寫入 URL。

### Source 目錄結構

```
content/
  posts/
    2026/
      graylog-install.md
      postfix-relay.md
    2025/
      linux-init.md
```

### 希望輸出 URL

```
/posts/2026/07/graylog-install/
/posts/2026/07/postfix-relay/
/posts/2025/03/linux-init/
```

### 問題說明

- Source 路徑：`content/posts/年/文章.md`（無月份層）
- URL 設計：`/posts/年/月/文章/`（有月份）
- Hugo 預設行為：將根據目錄層級產生 URL，**不會自行加入月份**
- 月份來源：Frontmatter 中的 `date` 欄位

---

## 二、必須設定：Permalinks

必須在 `hugo.toml` 加入以下設定，URL 才會包含月份。

### hugo.toml 設定（新版 Hugo v0.112+）

```toml
[permalinks]
  [permalinks.page]
    posts = '/posts/:year/:month/:slug/'
```

### hugo.toml 設定（舊版相容寫法）

```toml
[permalinks]
  posts = '/posts/:year/:month/:slug/'
```

### 可用參數說明

| 參數 | 說明 | 範例 |
|---|---|---|
| `:year` | Frontmatter date 年份 | `2026` |
| `:month` | Frontmatter date 月份（2 位） | `07` |
| `:slug` | Frontmatter slug 或檔名 | `graylog-install` |
| `:day` | Frontmatter date 日 | `13` |
| `:title` | 文章標題（不建議用） | - |

**建議：始終在 Frontmatter 明討設定 `slug`，不依賴檔名自動生成。**

---

## 三、完整 hugo.toml 必需設定清單

```toml
baseURL = 'https://sungshu.github.io/'
languageCode = 'zh-tw'
title = 'sungshu 手札筆記本'
theme = 'stack'

[permalinks]
  [permalinks.page]
    posts = '/posts/:year/:month/:slug/'

[params]
  mainSections = ['posts']

[pagination]
  pagerSize = 10
```

---

## 四、驗證方法

Hugo Build 後，確認輸出目錄結構：

```bash
hugo --minify
ls public/posts/2026/07/
```

应見到：

```
graylog-install/
postfix-relay/
```

若目錄下缺少月份，代表 `[permalinks]` 設定未生效。

---

## 五、相關文件

- `04-hugo-structure.md` — Hugo 目錄結構
- `06-content-url-design.md` — URL 設計規則
- `11-content-frontmatter.md` — Frontmatter 標準（含 date/slug 欄位）

---

## 附記

- 此文件為 `06-content-url-design.md` 的工程實作補充
- Source 路徑與 URL 之間的月份差異，必須透過 `[permalinks]` 解決，不可透過調整目錄結構解決
