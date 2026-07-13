# sungshu 手札筆記本 Hugo Blog

## AI Context & Engineering Specification

![Project Status](https://img.shields.io/badge/status-Engineering%20Specification%20v1.1-blue)

本 Repository 是 **sungshu 手札筆記本** 部落格專案的 AI Context 與工程規格文件。

用途：

* 提供 AI 助手完整專案背景
* 提供協作者架構說明
* 保存技術決策紀錄
* 定義開發與維護規範

---

# 重要說明

## 這不是 Hugo 教學專案

本專案不是單純建立一個 Hugo 靜態網站。

真正目標：

> 建立一套私人化技術部落格平台，使用體驗接近 Blogger / Pixnet，但保留完整資料控制、版本管理、SEO 與長期維護能力。

---

# Project Goal

## 專案目的

建立個人技術部落格平台：

```
Blogger / Pixnet 使用便利性

+

Git 版本控制

+

Hugo 靜態網站效能

+

完整資料自主權
```

希望達成：

* 後台文章管理
* 草稿管理
* 分類與標籤
* SEO 控制
* 自訂網站功能
* 長期維護

---

# Core Design Philosophy

本專案核心理念：

## 使用者不應該需要成為 Hugo 工程師才能寫文章

文章流程應接近：

```
登入後台

↓

建立文章

↓

編輯內容

↓

儲存草稿

↓

發布

↓

網站更新
```

而不是：

```
VS Code

↓

Markdown

↓

Git Commit

↓

Hugo Build
```

Git 與 Hugo 是底層架構，不是使用者操作入口。

---

# Technology Stack

目前技術架構：

| Component        | Technology            |
| ---------------- | --------------------- |
| Static Generator | Hugo                  |
| Theme            | Stack Theme           |
| CMS              | Decap CMS             |
| Source Control   | GitHub                |
| Hosting          | GitHub Pages          |
| Authentication   | GitHub OAuth          |
| Analytics        | GA4 / Umami (Planned) |
| Comment System   | Waline (Planned)      |

---

# System Architecture

完整資料流：

```
                    Author

                      |
                      v

              Decap CMS Admin

              /admin

                      |

                      v

              GitHub OAuth

                      |

                      v

        blog-source (Private Repository)

        - Hugo Source
        - Markdown Content
        - Drafts
        - Theme
        - Configuration

                      |

                      v

              Hugo Build

                      |

                      v

        sungshu.github.io

        Public Website

                      |

                      v

                  Visitors
```

---

# GitHub Repository Design

目前 Repository 分工：

| Repository        | Purpose                                | Visibility |
| ----------------- | -------------------------------------- | ---------- |
| sungshu.github.io | 公開網站輸出                                 | Public     |
| blog-source       | Hugo 原始碼與內容                            | Private    |
| blog              | Hugo Playground / 測試環境                 | Public     |
| blog-docs         | 詳細技術文件                                 | Private    |
| blog-ai-context   | AI Context / Engineering Specification | Public     |

---

# Important Architecture Decisions

以下為已確認架構，不應任意修改。

---

## 1. Source Repository 必須保持 Private

`blog-source`

包含：

* 原始 Hugo 專案
* Markdown 文章
* 草稿
* CMS 設定
* Theme 修改
* Site Configuration

不可直接公開。

---

## 2. Public Repository 只提供網站

`sungshu.github.io`

用途：

* GitHub Pages Hosting
* 公開 HTML
* CSS
* JavaScript
* 靜態資源

不是：

* 原始碼庫
* 草稿庫
* CMS Repository

---

## 3. Hugo 不是 CMS

Hugo 負責：

```
Markdown

↓

Hugo Build

↓

HTML
```

文章管理：

```
Decap CMS
```

---

## 4. 不回 Blogger / Pixnet

本專案目的：

建立自己的平台。

不以：

* Blogger
* Pixnet
* WordPress

作為最終方案。

---

# Content Structure

文章來源：

```
content/posts/
```

設計：

```
posts/

 ├── 2026/

 │     ├── article-a.md

 │     └── article-b.md
```

---

URL 設計：

```
/posts/2026/07/article-name/
```

Source Path 與 URL Path 分離。

透過 Hugo permalink 控制。

---

# CMS Design

使用：

```
Decap CMS
```

登入：

```
https://sungshu.github.io/admin/
```

流程：

```
User

↓

CMS Login

↓

GitHub OAuth

↓

Private Repository Access

↓

Create / Edit Content

↓

Commit

↓

Deploy
```

---

# Development Workflow

## CMS Workflow

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

## Git Workflow

適合：

* Theme 修改
* Hugo 設定
* 高階調整

流程：

```
Local Development

↓

Git Commit

↓

Push

↓

Build

↓

Deploy
```

---

# Feature Roadmap

## Phase 1 - Foundation

完成：

* Repository Design
* Hugo Architecture
* Theme Selection
* CMS Planning

---

## Phase 2 - Publishing System

目標：

* Hugo Source 建立
* Stack Theme 整合
* Decap CMS
* Deploy Pipeline

---

## Phase 3 - Blog Features

規劃：

* Search
* Comment
* Like
* View Counter
* RSS
* Sitemap

---

## Phase 4 - Operation

規劃：

* GA4
* Google Search Console
* SEO Optimization
* Advertisement
* Reward System

---

# Reference Websites

參考網站用途：

研究：

* UI Layout
* Blog Features
* User Experience
* Plugin Integration

注意：

Reference Website 不代表一定導入。

---

# AI Working Rules

任何 AI 參與本專案時：

## 必須：

1. 先理解架構，再提出修改。
2. 保持 Repository 分工。
3. 尊重已完成決策。
4. 區分「已完成」與「規劃中」。

---

## 禁止：

不要自行：

* 改成 WordPress
* 改成 Blogger
* 公開 Source Repository
* 移除 Decap CMS
* 將 Hugo 當 CMS
* 合併 Public / Private Repository

---

# Document Structure

完整文件：

| File                           | Purpose       |
| ------------------------------ | ------------- |
| 01-project-overview.md         | 專案目的          |
| 02-architecture.md             | 系統架構          |
| 03-github-repository-design.md | Repository 設計 |
| 04-hugo-structure.md           | Hugo 結構       |
| 05-cms-design.md               | CMS 設計        |
| 06-content-url-design.md       | URL 設計        |
| 07-theme-customization.md      | Theme 修改      |
| 08-feature-roadmap.md          | 功能規劃          |
| 09-reference-websites.md       | 參考網站          |
| 10-ai-handover.md              | AI 交接         |
| 11-content-frontmatter.md      | 文章格式規範        |
| 12-hugo-config-reference.md    | Hugo 設定       |
| 13-cms-authentication.md       | CMS 認證        |
| 14-deployment-design.md        | 部署設計          |
| 15-content-workflow.md         | 文章流程          |
| 16-maintenance-guide.md        | 維護指南          |

---

# Recommended Reading Order

## New AI / New Developer

請依序閱讀：

```
README.md

↓

01-project-overview.md

↓

02-architecture.md

↓

03-github-repository-design.md

↓

05-cms-design.md

↓

10-ai-handover.md

↓

13-cms-authentication.md

↓

14-deployment-design.md

↓

15-content-workflow.md
```

---

# Current Status

目前狀態：

```
Architecture Design

        ✅

Engineering Specification v1.1

        ✅

Implementation

        ⏳
```

---

# Next Development Stage

下一階段：

1. 建立 blog-source
2. 建立 Hugo 基礎架構
3. 整合 Stack Theme
4. 建立 Decap CMS
5. 建立 GitHub Actions
6. 完成網站部署
7. 逐步加入 Blog 功能

---

# Version

Current:

```
v1.1 Engineering Specification
```

Project:

```
sungshu 手札筆記本
```

Purpose:

```
Private Technical Blog Platform
```
