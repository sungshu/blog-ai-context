# 15 - 內容撰寫與發佈流程

> 版本：v1.1
> 建立日期：2026-07-13
> 狀態：已確認，可直接遵守

---

## 一、兩種撰寫模式

### 模式 A：Decap CMS （主要方式）

適合：日常撰寫、不熄就發佈的文章

### 模式 B：直接編輯文件 （備用方式）

適合：大量文章移轉、技術性調整、釞正 Frontmatter

---

## 二、模式 A：Decap CMS 流程

```
1. 開啟 https://sungshu.github.io/admin
2. GitHub OAuth 登入
3. 點擊 New Post
4. 填入標題、Slug、日期、內容
5. 選擇狀態：
   - Draft = 草稿，不公開
   - Published = 立即觸發 Build
6. 點擊 Publish / Save
7. Decap CMS 自動 commit 到 blog-source
8. GitHub Actions 自動觸發 Hugo Build
9. 網站更新（通常 1~3 分鐘）
```

---

## 三、模式 B：直接編輯文件流程

```bash
# 1. Clone blog-source
git clone git@github.com:sungshu/blog-source.git
cd blog-source

# 2. 建立新文章
mkdir -p content/posts/2026
touch content/posts/2026/graylog-install.md

# 3. 編輯內容（加入 Frontmatter + 正文）
vim content/posts/2026/graylog-install.md

# 4. 專屬預覽（可選）
hugo server -D

# 5. Commit
git add content/posts/2026/graylog-install.md
git commit -m "post: add graylog-install article"
git push origin main

# GitHub Actions 自動觸發
```

---

## 四、文章資料夾命名規則

```
content/posts/
  2026/
    graylog-install.md        ← 正確
    postfix-relay-config.md   ← 正確
    linux-init.md             ← 正確

  錯誤範例：
    Graylog Install.md        ← 不要大寫或空格
    graylog安裝.md          ← 不要中文
```

規則：
- 檔名全小寫英文
- 字詞之間用 `-`
- 不需包含日期（日期已在 Frontmatter 中的 `date`）

---

## 五、草稿 vs 發佈強制規則

| Frontmatter `draft` | 效果 |
|---|---|
| `draft: true` | Hugo Build 不產生頁面，訪客看不到 |
| `draft: false` | Hugo Build 產生頁面，公開可覽 |

**永遠不要 commit `draft: true` 且期望公開的文章。**

---

## 六、圖片管理

圖片資料夾：`blog-source/static/images/年/`

```
static/
  images/
    2026/
      graylog-dashboard.png
      postfix-flow.png
```

Frontmatter 內引用：

```yaml
image: "/images/2026/graylog-dashboard.png"
```

正文內引用：

```markdown
![Graylog Dashboard](/images/2026/graylog-dashboard.png)
```

---

## 七、相關文件

- `11-content-frontmatter.md` — Frontmatter 標準
- `12-hugo-config-reference.md` — Hugo permalink 設定
- `13-cms-authentication.md` — Decap CMS 登入流程
- `14-deployment-design.md` — GitHub Actions 部署
