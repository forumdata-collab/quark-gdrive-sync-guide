# 夸克網盤 × Google Drive 自動同步 101 教學

> 開放 API 實戰指南 — 用夸克網盤與 Google Drive 的官方 API，在雲端 VM 上做指定資料夾的自動化同步（單向 / 雙向）。

**線上頁面** → https://forumdata-collab.github.io/quark-gdrive-sync-guide/

## 這份指南講什麼

| 章節 | 內容 |
|---|---|
| 架構原理 | 為何用「VM 中轉」做同步、自動對比機制、零成本 |
| 事前準備 | 夸克網盤官方 Skill 安裝、GDrive OAuth Client 開設 |
| 實戰 | 單向同步腳本、雙向同步腳本、Diff 判定規則 |
| cron 排程 | 定時自動同步、log 管理、防重入 |
| 系統負荷 | 對 VM / API 配額的實際影響 |
| 常見坑 | 軟刪除、時間戳仲裁、hash 二次確認、配額限制 |
| Cheatsheet | 常用指令速查 |

## 技術摘要

- **同步媒介**：夸克網盤官方開放 API + Google Drive API v3
- **仲裁規則**：`modifiedTime` 是唯一仲裁；內容 hash 做二次確認；刪除統一用「軟刪除」（避免雙向刪除互相覆寫）
- **執行環境**：雲端 VM（本指南實測於 Oracle Cloud ARM instance）
- **成本**：兩個平台免費額度內完全免費
- **自動化**：`cron` 定時跑 sync 腳本，支援單向、雙向兩種模式

## 目錄結構

```
quark-gdrive-sync-guide/
├── index.html   # 教學頁（GitHub Pages 單頁應用）
└── README.md    # 本文件
```

## 本地開發 / 預覽

```bash
# 改完 index.html 直接 push 就會自動更新 Pages
git add index.html && git commit -m "update guide" && git push

# 本地預覽
python3 -m http.server 8080
# → http://localhost:8080
```

## 相關兄弟指南

- [Hermes CF 教學](https://forumdata-collab.github.io/hermes-cf-guide/)
- [Hermes MP3 教學](https://forumdata-collab.github.io/hermes-mp3-guide/)
- [Hermes Oracle 教學](https://forumdata-collab.github.io/hermes-oracle-guide/)

## 授權

內容以 CC BY-NC 形式分享，歡迎引用並註明出處。