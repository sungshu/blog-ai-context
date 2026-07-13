# 16 - 長期維護指南

> 版本：v1.1
> 建立日期：2026-07-13
> 目標：支援長期經營 5~10 年

---

## 一、定期維護項目

### 每次架構決策後（立即）

- [ ] 更新 `blog-ai-context` 相關文件
- [ ] 更新 `CHANGELOG.md`（新增版本號與變更摘要）

### 每月

- [ ] 檢查 Hugo 是否有新版本，評估是否升版
- [ ] 檢查 Stack Theme 是否有更新（若使用 submodule）
- [ ] 確認 GitHub Actions workflow 尚在正常運行

### 每年

- [ ] 審查 `08-feature-roadmap.md` Phase 進度
- [ ] 更新 `10-ai-handover.md` 的現狀說明
- [ ] 重新阅讀 `blog-ai-context` 所有文件，確認與實際狀態一致

---

## 二、Stack Theme 升版作業程序

```bash
# 1. 查詢目前版本
cd blog-source
git submodule status

# 2. 更新 submodule
git submodule update --remote themes/stack

# 3. 對新版本進行本地測試
hugo server
# 檢查版面是否對罎

# 4. 檢查客製化 CSS 是否受影響
# 參考 07-theme-customization.md 記錄的更改

# 5. 確認沒問題後 commit
git add themes/stack
git commit -m "chore: update stack theme to latest"
git push origin main
```

**重要：升版後必須檢查 `07-theme-customization.md` 中記錄的所有客製化更改是否依然生效。**

---

## 三、Hugo 升版作業程序

```yaml
# 在 .github/workflows/deploy.yml 中
# 更改版本號
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v3
  with:
    hugo-version: '0.147.0'  # 指定固定版本，不要用 'latest'
    extended: true
```

**建議：將 `latest` 改為固定版本號，避免 Hugo 煕聲升版導致 Build 失敗。**

---

## 四、blog-ai-context 同步規範

每次發生以下情況，必須同步更新 `blog-ai-context`：

| 情況 | 應更新的文件 |
|---|---|
| 常務架構決策 | 相關文件 + CHANGELOG |
| Hugo 升版 | `12-hugo-config-reference.md` + CHANGELOG |
| Stack Theme 升版 | `07-theme-customization.md` + CHANGELOG |
| 新增功能（搜尋/評論） | `08-feature-roadmap.md` + 相關文件 |
| 換 AI 協作者 | `10-ai-handover.md` |

---

## 五、風險負負表

| 風險 | 細節 | 緩解方式 |
|---|---|---|
| Decap CMS 停止維護 | 社群相對小眾 | 內容是純 Markdown，可移至其他 CMS |
| Stack Theme 升版相容 | 客製化 CSS 可能被覆蓋 | 在 `07` 記錄每次改了哪個原始檔 |
| GitHub Pages 變動 | 機率小 | 静態檔案可將移至 Cloudflare Pages |
| GitHub Actions 版本座 | action 可能謜棄 | 定期確認 `uses: action@v?` 版本 |

---

## 六、第二陸跳選項（若需將來迀移）

若 GitHub Pages 變動或需升級：

| 選項 | 說明 |
|---|---|
| Cloudflare Pages | 免費，支援 Hugo，可直接連接 GitHub |
| Netlify | 免費方案加費用模式，選項豐富 |
| 自架 VPS | 最大彈性，但維護負擔增加 |

選擇 Cloudflare Pages 过渡成本最低，因為可直接連接 GitHub Repo，不需要修改 Workflow。

---

## 七、相關文件

- `07-theme-customization.md` — Theme 客製化記錄
- `08-feature-roadmap.md` — 功能路線圖
- `10-ai-handover.md` — AI 協作手册
- `14-deployment-design.md` — 部署設計
