# sungshu 手札筆記本 Project Context

目的:
建立私人技術 Blog 平台。

類似:
Blogger / Pixnet

不是:
Hugo 教學專案


核心架構:

blog-source
Private
(原始碼、文章、CMS)

↓

GitHub Actions

↓

sungshu.github.io
Public
(網站輸出)


不可變更:

1. 不回 Blogger
2. 不使用 WordPress
3. 不公開 source
4. Hugo 是 Generator，不是 CMS
5. Decap CMS 是使用者入口
