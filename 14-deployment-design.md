# 14 - 部署設計（GitHub Actions）

> 版本：v1.1
> 建立日期：2026-07-13
> 狀態：已確認架構，待實作

---

## 一、部署目標

| 項目 | 說明 |
|---|---|
| Source Repo | `sungshu/blog-source`（Private）|
| 輸出 Repo | `sungshu/sungshu.github.io`（Public）|
| 觸發條件 | `blog-source` main branch 有新 commit |
| Build 工具 | Hugo |
| 部署工具 | GitHub Actions |

---

## 二、完整 Workflow 檢視圖

```
blog-source（Private）
  │
  ├── 內容變更 or Decap CMS Commit
  │
  └── 觸發 GitHub Actions
              │
              ├── Checkout blog-source
              ├── Setup Hugo
              ├── Hugo Build
              ├── 輸出: public/
              │
              └── Push public/ → sungshu.github.io（Public）
                              │
                              └── GitHub Pages 自動更新網站
```

---

## 三、GitHub Actions Workflow 檔案

檔案位置：`blog-source/.github/workflows/deploy.yml`

```yaml
name: Hugo Build and Deploy

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout blog-source
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy to sungshu.github.io
        uses: peaceiris/actions-gh-pages@v4
        with:
          personal_token: ${{ secrets.DEPLOY_TOKEN }}
          external_repository: sungshu/sungshu.github.io
          publish_branch: main
          publish_dir: ./public
```

---

## 四、必須設定項目

### 4.1 DEPLOY_TOKEN 設定

因為要 push 到另一個 Repository，需要 Personal Access Token：

1. 前往：GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. 建立新 Token，選擇 Scope：

| Scope | 原因 |
|---|---|
| `repo` | 存取 Private `blog-source` |
| `workflow` | 有權限修改 `.github/workflows/` |
| `public_repo` | Push 到 Public `sungshu.github.io` |

3. 建立完成後，將 Token 複製
4. 前往：`blog-source` → Settings → Secrets and variables → Actions
5. 新增 Secret：
   - Name：`DEPLOY_TOKEN`
   - Value：上面複製的 Token

### 4.2 Stack Theme 作為 Git Submodule

若 Stack Theme 透過 submodule 引入，Workflow 的 `submodules: true` 參數必須保留：

```bash
# blog-source 下初始化 submodule
git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/stack
git commit -m "add stack theme as submodule"
```

---

## 五、部署驗證

Workflow 完成後，確認以下項目：

```bash
# 1. 檢查 GitHub Actions 所有 steps 都成功
#    blog-source → Actions tab → 最新一次 workflow run

# 2. 確認網站更新
https://sungshu.github.io

# 3. 檢查 sungshu.github.io 的 commit 記錄
#    應對應 blog-source 的每一次 commit
```

---

## 六、常見問題

| 問題 | 原因 | 解決方式 |
|---|---|---|
| Build 失敗 | Hugo 版本不相容 | 指定固定版本，如 `hugo-version: '0.147.0'` |
| submodule 空白 | Checkout 沒包含 submodule | 確認 `submodules: true` |
| Push 權限被拒 | Token 設定錯誤 | 重新確認 `DEPLOY_TOKEN` 的 scope |
| `/admin` 沒有輸出 | `static/admin/` 位置錯誤 | 確認 `admin/` 在 `blog-source/static/admin/` |

---

## 七、相關文件

- `02-architecture.md` — 整體架構
- `03-github-repository-design.md` — Repo 分工說明
- `13-cms-authentication.md` — Decap CMS 認證流程
