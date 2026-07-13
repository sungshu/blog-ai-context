# 11 - 文章 Frontmatter 標準

> 版本：v1.1
> 建立日期：2026-07-13
> 狀態：已確認必須遵守

---

## 一、Frontmatter 標準格式

所有文章必須使用 YAML Frontmatter，位於檔案最頂部。

### 標準範例

```yaml
---
title: "Graylog 5 安裝指南 - Rocky Linux 9"
date: 2026-07-13T14:00:00+08:00
slug: graylog-install-rocky9
description: "詳細說明如何在 Rocky Linux 9 上安裝 Graylog 5 日誌分析平台"
draft: false
tags:
  - graylog
  - rocky-linux
  - logging
categories:
  - 系統管理
image: ""
---
```

---

## 二、欄位說明

### 必需欄位

| 欄位 | 即型態 | 說明 |
|---|---|---|
| `title` | String | 文章標題，顯示於瀏覽器標籤與文章頁 |
| `date` | DateTime | 發佈日期，**必須含時區 `+08:00`**，用於產生 URL 中月份 |
| `slug` | String | URL 路徑第三層。建議使用英文 + `-`，不含空格或中文 |
| `draft` | Boolean | `true` = 草稿，不發佈。`false` = 正式發佈 |

### 建議欄位

| 欄位 | 類型 | 說明 |
|---|---|---|
| `description` | String | 文章摘要，顯示於文章列表與 SEO meta |
| `tags` | List | 標籤，建議 3~5 個 |
| `categories` | List | 分類，建議 1~2 個 |
| `image` | String | 封面圖路徑，路徑相對於 `static/`，可留空 |

### 不建議欄位

| 欄位 | 說明 |
|---|---|
| `url` | 不要手動設定，由 `slug` + `[permalinks]` 控制 |
| `weight` | 除非需要特殊排序，否則不加 |

---

## 三、`date` 欄位重要性

`date` 是生成 URL 月份的來源。

```yaml
# 正確：含時區
date: 2026-07-13T14:00:00+08:00

# 可接受：但可能導致時區誤差
date: 2026-07-13

# 錯誤：空白或未填
# 會導致 Hugo 無法產生對應 URL
```

**建議始終使用 ISO 8601 包含時區的格式。**

---

## 四、`slug` 命名規則

```
正確：
graylog-install-rocky9
postfix-relay-config
linux-network-routing

錯誤：
Graylog Install Rocky9    ← 不要大寫
Graylog安裝指南           ← 不要中文
graylog_install           ← 不要底線
```

規則：
- 全小寫英文
- 字詞之間用 `-` 連接
- 不含空格、中文、底線
- 建議長度 3~6 個字詞

---

## 五、完整 URL 產生過程

```
Frontmatter:
  date: 2026-07-13T14:00:00+08:00
  slug: graylog-install-rocky9

hugo.toml:
  [permalinks]
    posts = '/posts/:year/:month/:slug/'

輸出 URL:
  /posts/2026/07/graylog-install-rocky9/
```

---

## 六、相關文件

- `06-content-url-design.md` — URL 設計規則
- `12-hugo-config-reference.md` — Hugo permalink 設定
- `13-cms-authentication.md` — Decap CMS config.yml 中的 fields 對應
- `15-content-workflow.md` — 文章撰寫流程
