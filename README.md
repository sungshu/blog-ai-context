# sungshu 手札筆記本 Hugo Blog

## AI Context & Engineering Specification

本 Repository 用於保存 **sungshu 手札筆記本** 部落格專案的：

* 專案背景
* 架構設計
* 技術決策
* GitHub Repository 規劃
* Hugo 設定規範
* CMS 工作流程
* 部署流程
* 長期維護規範

本 Repository 的目的不是建立 Hugo 教學文件，而是讓 AI、協作者或未來維護者能快速理解整個系統設計。

---

# Project Context

## 專案目標

建立一套私人化技術部落格平台。

使用體驗希望接近：

* Blogger
* Pixnet

但保留：

* Git 版本管理
* Markdown 文章格式
* SEO 控制能力
* 自訂網站功能
* 長期維護能力

---

## 核心理念

本專案希望達成：

```
簡單發文體驗
+
完整資料控制
+
工程化管理方式
```

使用者不需要直接操作 Git 或 Hugo，
而是透過 CMS 後台完成文章管理。

---

# Technology Stack

目前架構：

```
Hugo
+
Stack Theme
+
Decap CMS
+
GitHub
+
GitHub Pages
```

主要用途：

| 技術           | 用途               |
| ------------ | ---------------- |
| Hugo         | 靜態網站產生器          |
| Stack Theme  | Blog UI / Layout |
| Decap CMS    | 後台文章管理           |
| GitHub       | 原始碼與版本控制         |
| GitHub Pages | 公開網站部署           |

---

# Architecture Overview

整體資料流程：

```
              Writer
                |
                v
        Decap CMS (/admin)
                |
                v
        GitHub OAuth
                |
                v
      blog-source (Private)
                |
                |
          Hugo Build
                |
                v
      sungshu.github.io
             (Public)
                |
                v
            Visitors
```

---

# Repository Design

目前 Repository 分工：

| Repository        | Purpose                                | Permission |
| ----------------- | -------------------------------------- | ---------- |
| sungshu.github.io | 公開網站輸出                                 | Public     |
| blog-source       | Hugo Source / Content / CMS            | Private    |
| blog              | Hugo Playground / 測試區                  | Public     |
| blog-docs         | 詳細文件與維運紀錄                              | Private    |
| blog-ai-context   | AI Context / Engineering Specification | Public     |

---

# Important Design Decisions

以下為不可隨意變更的架構決策。

## Source 必須保持 Private

原因：

包含：

* 草稿文章
* 未發布內容
* Hugo 設定
* CMS 設定
* Theme 修改

不可將完整 source 直接公開。

---

## Public Repository 只放網站輸出

`sungshu.github.io`

用途：

* GitHub Pages Hosting
* 公開 HTML / CSS / JS
* 訪客瀏覽

不是：

* 原始文章庫
* 草稿庫
* CMS 資料庫

---

## Hugo 不是 CMS

Hugo 負責：

```
Markdown
        |
        v
Hugo Build
        |
        v
HTML
```

文章管理入口：

```
Decap CMS
```

---

## 不回 Blogger / Pixnet

本專案目的就是建立自己的平台。

不以：

* Blogger
* Pixnet
* WordPress

作為最終架構。

---

# Content Design

文章來源：

```
content/posts/
```

規劃：

```
posts/
 ├── 2026/
 │    ├── article-1.md
 │    └── article-2.md
```

URL 設計：

```
/posts/2026/07/article-name/
```

Source 結構與 URL 結構分離。

---

# CMS Workflow

文章流程：

```
Create Article

        |
        v

Draft

        |
        v

Review

        |
        v

Publish

        |
        v

Git Commit

        |
        v

Hugo Build

        |
        v

GitHub Pages Deploy
```

---

# Development Rules

## AI 工作原則

任何 AI 參與本專案時：

1. 先理解架構，再提出修改
2. 不自行替換技術方案
3. 不將 Private Source 改為 Public
4. 不移除 CMS 設計
5. 不改變核心 Repository 分工

---

# Recommended Reading Order

新加入 AI 或協作者請依序閱讀：

## Phase 1 - Understand

1. README.md
2. 01-project-overview.md
3. 02-architecture.md
4. 03-github-repository-design.md

## Phase 2 - Implementation

5. 04-hugo-structure.md
6. 05-cms-design.md
7. 12-hugo-config-reference.md
8. 13-cms-authentication.md
9. 14-deployment-design.md

## Phase 3 - Operation

10. 15-content-workflow.md
11. 16-maintenance-guide.md

---

# Documentation Index

| Document                       | Description   |
| ------------------------------ | ------------- |
| 01-project-overview.md         | 專案目的與背景       |
| 02-architecture.md             | 系統架構          |
| 03-github-repository-design.md | Repository 規劃 |
| 04-hugo-structure.md           | Hugo 目錄設計     |
| 05-cms-design.md               | Decap CMS 設計  |
| 06-content-url-design.md       | URL 規劃        |
| 07-theme-customization.md      | Theme 客製化     |
| 08-feature-roadmap.md          | 功能 Roadmap    |
| 09-reference-websites.md       | 參考網站          |
| 10-ai-handover.md              | AI 交接說明       |
| 11-content-frontmatter.md      | 文章格式規範        |
| 12-hugo-config-reference.md    | Hugo 設定規範     |
| 13-cms-authentication.md       | CMS 認證流程      |
| 14-deployment-design.md        | 部署設計          |
| 15-content-workflow.md         | 文章流程          |
| 16-maintenance-guide.md        | 維護指南          |

---

# Current Status

目前階段：

```
Architecture Design
        |
        v
Engineering Specification v1.1
        |
        v
Implementation Phase
```

下一階段：

* 建立 blog-source
* 完成 Hugo 基礎架構
* 整合 Stack Theme
* 建立 Decap CMS
* 建立 Deployment Pipeline
* 完成網站功能開發

---

# Version

Current Version:

```
v1.1 Engineering Specification
```

Last Update:

2026
