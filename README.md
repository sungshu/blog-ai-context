# sungshu 手札筆記本 Hugo Blog
## AI Context & Engineering Specification
![Project Status](https://img.shields.io/badge/status-Engineering%20Specification%20v1.1-blue)
> 本 Repository 是 **sungshu 手札筆記本** 部落格專案的 AI Context 與工程規格文件。
> 用於讓 AI、協作者與未來維護者快速理解專案目的、架構設計、技術決策與開發規範。

---

# 1. Project Identity

## 這是什麼？

`sungshu 手札筆記本` 是一個：

> 私人化技術部落格平台工程專案。

目標是建立一個接近：

* Blogger
* Pixnet

的文章管理體驗，

但保留：

* 完整資料控制
* Git 版本管理
* SEO 自主控制
* 靜態網站效能
* 長期維護能力

---

## 這不是什麼？

本專案：

不是：

* Hugo 教學範例
* 單純 Markdown 筆記庫
* GitHub Pages Demo
* Theme 展示網站

也不是要重新建立：

* Blogger
* Pixnet
* WordPress

而是建立自己的技術 Blog 平台。

---

# 2. Core Concept

## 使用者體驗目標

作者應該可以：

```
開啟後台

↓

登入

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
開啟 VS Code

↓

修改 Markdown

↓

Git Commit

↓

Hugo Build
```

---

## 架構理念

Hugo 是：

```
Static Site Generator
```

不是：

```
Content Management System
```

因此：

* Hugo 負責網站生成
* Decap CMS 負責文章管理

---

# 3. Technology Stack

目前技術選擇：

| Component        | Technology   | Status   |
| ---------------- | ------------ | -------- |
| Static Generator | Hugo         | Selected |
| Theme            | Stack Theme  | Selected |
| CMS              | Decap CMS    | Selected |
| Source Control   | GitHub       | Selected |
| Hosting          | GitHub Pages | Selected |
| Authentication   | GitHub OAuth | Design   |
| Analytics        | GA4 / Umami  | Planned  |
| Comment System   | Waline       | Planned  |
| Search           | TBD          | Planned  |

---

# 4. System Architecture

完整資料流程：

```
                    Author

                       |
                       v

                Decap CMS

                /admin

                       |
                       v

               GitHub OAuth

                       |
                       v

        blog-source (Private Repository)

        - Hugo Source
        - Markdown Content
        - Draft Articles
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

# 5. GitHub Repository Design

目前 Repository 分工：

| Repository        | Purpose                                | Visibility |
| ----------------- | -------------------------------------- | ---------- |
| sungshu.github.io | 公開網站輸出                                 | Public     |
| blog-source       | Hugo 原始碼、文章、CMS                        | Private    |
| blog              | Hugo Playground / 測試區                  | Public     |
| blog-docs         | 詳細技術文件                                 | Private    |
| blog-ai-context   | AI Context / Engineering Specification | Public     |

---

# 6. Important Architecture Decisions

以下為已確認決策。

## 6.1 Source Repository 保持 Private

`blog-source`

包含：

* Hugo source
* content
* draft
* configuration
* theme customization
* CMS configuration

不可直接公開。

---

## 6.2 Public Repository 只提供網站

`sungshu.github.io`

用途：

* GitHub Pages Hosting
* 公開網站內容

不包含：

* 原始 Hugo 專案
* 私人文章草稿
* CMS source

---

## 6.3 Decap CMS 是文章入口

文章管理：

```
Decap CMS

↓

Git Commit

↓

Hugo Build

↓

Deploy
```

---

## 6.4 不改變核心技術方向

除非重新討論，不應自行替換：

* Hugo
* Stack Theme
* Decap CMS
* GitHub Pages
* Private Source Repository

---

# 7. Content Structure

文章來源：

```
content/posts/
```

目前規劃：

```
posts/

 ├── 2026/

 │    ├── article-a.md

 │    └── article-b.md
```

---

URL 設計：

```
/posts/YYYY/MM/article-name/
```

Source Path 與 URL Path 分離。

透過 Hugo permalink 控制。

---

# 8. CMS Workflow

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

# 9. Feature Roadmap

## Phase 1 - Foundation

完成：

* Architecture Design
* Repository Planning
* Hugo Design
* CMS Design

---

## Phase 2 - Implementation

目標：

* 建立 blog-source
* Hugo 初始化
* Stack Theme
* Decap CMS
* GitHub Actions

---

## Phase 3 - Blog Features

規劃：

* Search
* Comment
* Like
* View Count
* RSS
* Sitemap

---

## Phase 4 - Operation

規劃：

* Google Analytics 4
* Google Search Console
* SEO Optimization
* Advertisement
* Reward System

---

# 10. Reference Website Rules

參考網站用途：

研究：

* Layout
* UI
* Feature
* User Experience

注意：

參考網站不代表：

* 已採用
* 已部署
* 必須實作

---

# 11. AI Working Rules

任何 AI 參與本專案：

## 必須：

1. 先閱讀 README 與相關文件。
2. 理解架構後再修改。
3. 區分：

   * 已完成
   * 設計中
   * 規劃中
4. 保留既有架構決策。

---

## 禁止：

不可自行：

* 改成 Blogger
* 改成 WordPress
* 公開 blog-source
* 移除 Decap CMS
* 將 Hugo 當 CMS
* 合併 Public / Private Repository

---

# 12. Current Status

目前狀態：

## Documentation

完成：

```
Engineering Specification v1.1
```

狀態：

```
Completed
```

---

## Implementation

狀態：

```
In Progress
```

尚未完成：

* Hugo Production Source
* CMS Deployment
* Full Website Feature Integration

---

# 13. Documentation Index

| File                           | Description   |
| ------------------------------ | ------------- |
| 01-project-overview.md         | 專案目的          |
| 02-architecture.md             | 系統架構          |
| 03-github-repository-design.md | Repository 設計 |
| 04-hugo-structure.md           | Hugo 結構       |
| 05-cms-design.md               | CMS 設計        |
| 06-content-url-design.md       | URL 設計        |
| 07-theme-customization.md      | Theme 客製化     |
| 08-feature-roadmap.md          | 功能規劃          |
| 09-reference-websites.md       | 參考網站          |
| 10-ai-handover.md              | AI 交接         |
| 11-content-frontmatter.md      | 文章格式          |
| 12-hugo-config-reference.md    | Hugo 設定       |
| 13-cms-authentication.md       | CMS 認證        |
| 14-deployment-design.md        | 部署設計          |
| 15-content-workflow.md         | 文章流程          |
| 16-maintenance-guide.md        | 維護指南          |

---

# 14. Recommended Reading Order

新 AI 或協作者：

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

# Version

Current Version:

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
