# 13 - CMS 認證與部署流程

> 版本：v1.1
> 建立日期：2026-07-13
> 狀態：已確認架構，待實作

---

## 一、架構澄清說明

### 混淡點（容易誤解）

`blog-source`（Private）內部有 `/admin` 目錄，但訪客是透過 `sungshu.github.io/admin`（Public）進入 CMS。

這兩者**不是同一個地方**，需要明確區分。

### 正確理解

```
blog-source（Private）
└── static/
    └── admin/
        ├── index.html      ← Decap CMS UI 組件
        └── config.yml      ← 設定：連接哪個 Repo、哪個 branch

Hugo Build 輸出 → sungshu.github.io（Public）
└── admin/
    ├── index.html      ← 訪客實際使用的畫面
    └── config.yml
```

---

## 二、完整登入流程

```
使用者登入
  ↓
開啟 https://sungshu.github.io/admin
  ↓
  Decap CMS UI（連接 GitHub 按鈕）
  ↓
  GitHub OAuth App
  ↓
  GitHub 登入頁（需要 sungshu 帳號）
  ↓
  OAuth token 達到瀏覽器
  ↓
  Decap CMS 讀寫 blog-source（Private）
  ↓
  編輯 / 儲存文章
  ↓
  Commit Markdown 到 blog-source/content/posts/
  ↓
  GitHub Actions 觸發
  ↓
  Hugo Build
  ↓
  Push 到 sungshu.github.io（Public）
```

---

## 三、GitHub OAuth App 設定步驟

### 3.1 建立 OAuth App

1. 前往：GitHub → Settings → Developer settings → OAuth Apps → New OAuth App
2. 填入以下資訊：

| 欄位 | 內容 |
|---|---|
| Application name | `sungshu-blog-cms` |
| Homepage URL | `https://sungshu.github.io` |
| Authorization callback URL | `https://sungshu.github.io/api/auth` |

3. 建立後取得 `Client ID` 與 `Client Secret`

> **注意**：Callback URL 依 Decap CMS 使用的 backend 配置而定。若使用 Netlify Identity backend 則不同。

### 3.2 必需的 Token Scope

OAuth App 自動授權經用戶同意的 scope。Decap CMS 需要以下權限：

| Scope | 用途 |
|---|---|
| `repo` | 存取 Private Repository （必需） |
| `user:email` | 讀取帳號信票（身份驗證用） |

---

## 四、Decap CMS config.yml 標準設定

檔案位置：`blog-source/static/admin/config.yml`

```yaml
backend:
  name: github
  repo: sungshu/blog-source
  branch: main

media_folder: static/images
public_folder: /images

collections:
  - name: posts
    label: 文章
    folder: content/posts
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
      - { label: 標題, name: title, widget: string }
      - { label: 發布日期, name: date, widget: datetime }
      - { label: Slug, name: slug, widget: string }
      - { label: 摘要, name: description, widget: text, required: false }
      - { label: 內容, name: body, widget: markdown }
      - { label: 標籤, name: tags, widget: list, required: false }
      - { label: 分類, name: categories, widget: list, required: false }
```

---

## 五、安全性考量

### 目前階段：僅 OAuth 防護

- `sungshu.github.io/admin` URL 公開可訪問
- 任何人可以開啟 CMS 登入頁
- 實際寫入需要 **GitHub OAuth 認證**
- GitHub 層級的權限控制已足夠
- **不建議加 HTTP Basic Auth**：容易影響 OAuth 認證流程

### 未來階段：若需增強

若未來遷移至自架 VPS 或有多位管理者：

| 方案 | 說明 |
|---|---|
| Cloudflare Access | Zero Trust ，免費方案，支援 GitHub 登入 |
| Cloudflare Zero Trust | 加入 IP 或 Email 白名單 |

---

## 六、相關文件

- `02-architecture.md` — 整體架構與資料流
- `05-cms-design.md` — CMS 設計紀錄
- `14-deployment-design.md` — GitHub Actions 部署設計

---

## 附記

- 此文件为 `05-cms-design.md` 的工程實作補充
- config.yml 中的 `repo: sungshu/blog-source` 指向 Private Repo，這是很多人容易誤設的地方
