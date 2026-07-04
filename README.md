# 記帳本 Expense Tracker

**[中文說明](#中文說明) · [English](#english)**

---

## 中文說明

一個為台灣使用者設計的免費記帳 App — 直接匯入財政部電子發票，自動分類、圖表分析、跨裝置同步。單一 HTML 檔案，不需要伺服器、不需要註冊帳號，你的消費資料**只存在你自己的裝置和你自己的私人 GitHub repo**，沒有任何第三方看得到。

### ✨ 功能

- **匯入電子發票**：把「財政部電子發票整合服務平台」匯出的 CSV 直接拖進來，自動去除重複與作廢發票
- **重複偵測**：匯入時若發現手動輸入的紀錄和發票「同日、同金額」，跳出對照清單讓你確認是否以發票取代，不會誤刪
- **自動分類**：依店家與品項名稱自動歸類（全聯→飲食、50嵐→飲品、中油→交通、負數折扣→紅利折扣…），13 種分類可自行調整
- **品項層級分類**：同一張發票裡的品項可以分屬不同分類（便當算飲食、飲料算飲品），圖表按品項精準拆分
- **總覽儀表板**：月支出、分類佔比圓餅圖（可點擊鑽取、含百分比）、每日趨勢、預算進度條
- **每月預算**：按分類設定，接近上限變黃、超支變紅
- **手動記帳與編輯**：現金消費快速輸入，支援多品項與金額自動加總；每筆紀錄都能事後編輯
- **美金記帳**：出國時選 US$ 輸入，用即時匯率換算成台幣入帳，**匯率在記錄當下鎖定**，之後不會變動
- **跨裝置同步**：透過你自己的私人 GitHub repo 同步手機和電腦，自動合併、刪除也會同步
- **中英雙語**：右上角一鍵切換
- **主題外觀**：四種主題色（靛藍/森林綠/暖陽橘/紫羅蘭）+ 深色模式（可跟隨系統）
- **資料自主**：隨時匯出 CSV / JSON 備份，資料 100% 屬於你

### 🚀 開始使用

#### 1. 部署 App（約 5 分鐘）

1. Fork 這個 repo（或建立新 repo 後上傳 `index.html`）
2. 到 repo 的 **Settings → Pages**，Source 選 **Deploy from a branch**，Branch 選 `main` / `root`，按 Save
3. 一兩分鐘後你的 App 就在 `https://你的帳號.github.io/repo名稱/`
4. 手機用 Safari/Chrome 打開網址 → **加入主畫面**，就像原生 App 一樣

> Repo 是公開的沒關係 — 裡面只有程式碼，你的消費資料不會上傳到這裡。

#### 2. 匯入你的電子發票

前提：你已經有載具（手機條碼），並在財政部平台完成歸戶。

1. 登入 [財政部電子發票整合服務平台](https://www.einvoice.nat.gov.tw/)
2. 到載具消費紀錄查詢，選擇月份，**匯出 CSV**
3. 打開 App → 「匯入發票」→ 把 CSV 拖進去，完成

建議每個月初匯出上個月的發票，一分鐘搞定整月記帳。已匯入的發票（相同發票號碼）會自動略過；如果有手動紀錄疑似和發票重複，會跳出清單讓你逐筆確認。

> **為什麼不能自動抓發票？** 財政部的發票查詢 API 自 2023 年起只開放公司申請（且需 ISO 27001 認證），個人無法使用，所以手動匯出 CSV 是目前個人能用的最佳方案。

#### 3. 設定跨裝置同步（選用）

1. 建立第二個 repo，例如 `expense-data`，設為 **Private**（這裡放你的資料）
2. GitHub → Settings → Developer settings → **Fine-grained personal access tokens** → 產生新 token：
   - Repository access：**Only select repositories** → 只勾 `expense-data`
   - Permissions → Contents：**Read and write**
3. 在 App 的「設定 → GitHub 雲端同步」填入 `你的帳號/expense-data` 和 token，按「立即同步」
4. 在每台裝置做一次同樣設定，之後隨時按同步就會自動合併兩邊的紀錄（含刪除）

> Token 只存在你裝置的瀏覽器裡，而且權限只限那一個私人 repo，就算外洩也碰不到你其他東西。

### 🔒 隱私

- 消費資料存在瀏覽器的 local storage，**不會**上傳到任何伺服器
- 同步功能使用**你自己的**私人 GitHub repo，只有你的帳號能存取
- 匯率查詢只發送「查詢 USD 匯率」的請求，不含任何個人資料
- 公開 repo 裡只有程式碼；請**不要**把發票 CSV 或 JSON 備份上傳到公開 repo

### ⚠️ 注意事項

- 資料綁定瀏覽器：清除瀏覽器資料會刪掉紀錄，請善用「備份 (JSON)」或 GitHub 同步
- 美金換算使用免費 API 的中間價（每 12 小時更新，離線時用上次匯率），與信用卡實際匯率會有 1–2% 差異，僅供參考
- 平台匯出的 CSV 為目前（2026）格式；若財政部改版導致匯入失敗，歡迎開 issue

---

## English

A free, privacy-first expense tracker built for people living in Taiwan. It imports your e-invoices (電子發票) directly from the Ministry of Finance platform, auto-categorizes every purchase, and gives you charts, budgets, and cross-device sync. The whole app is a single HTML file — no server, no signup. Your spending data lives **only on your own devices and in your own private GitHub repo**; no third party ever sees it.

### Background: how e-invoices work in Taiwan

Nearly every receipt in Taiwan is a government-registered e-invoice. If you set up a carrier barcode (載具, a QR code in your phone) and have cashiers scan it, all your invoices are stored under your account on the Ministry of Finance platform. That means your purchase history — store, date, amount, and even individual line items — already exists in one place; this app turns it into a personal expense tracker.

One catch: the MOF's invoice-query API has been restricted to ISO 27001-certified companies since 2023, so individuals can't pull invoices automatically. Instead, you log in to [einvoice.nat.gov.tw](https://www.einvoice.nat.gov.tw/) once a month, export your carrier history as a CSV, and drop it into this app. It takes about a minute.

### ✨ Features

- **E-invoice CSV import** — drag in the platform's export; voided invoices are skipped and previously-imported invoices are deduplicated by invoice number
- **Duplicate review** — if a manually-entered record shares the same date and amount as a newly imported invoice, a side-by-side confirmation list lets you decide which manual records to replace; nothing is deleted silently
- **Auto-categorization** — store and item names map to 13 categories (PX Mart → Food, 50嵐 → Drinks, CPC gas → Transport, negative discount lines → Rewards/Discount…), all adjustable afterward
- **Item-level categories** — line items on a single invoice can belong to different categories (the bento counts as Food, the milk tea as Drinks), and charts split amounts accordingly
- **Dashboard** — monthly total, category doughnut chart with percentages (click a slice to drill into matching records), daily spending trend, and budget progress bars
- **Monthly budgets** — per category; bars turn amber near the limit and red when over
- **Manual entry & editing** — quick form for cash spending with optional line items that auto-sum; every record can be edited later
- **USD entry for travel** — switch the amount field to US$, and the app converts to NT$ using a live exchange rate. **The rate is locked at the moment of recording** and never changes afterward; the original USD amounts stay visible for reference. Rates are cached for offline use.
- **Cross-device sync** — via your own private GitHub repo; records from all devices are merged automatically (deletions included, so removed records don't resurrect)
- **Bilingual UI** — one button toggles Mandarin ↔ English
- **Themes** — four accent colors (Indigo / Forest / Sunset / Violet) plus dark mode with follow-system option
- **Your data, always** — export everything as CSV or JSON at any time

### 🚀 Getting started

#### 1. Deploy the app (~5 minutes)

1. Fork this repo (or create a new one and upload `index.html`)
2. In the repo, go to **Settings → Pages**, set Source to **Deploy from a branch**, choose `main` / `root`, and save
3. After a minute or two your app is live at `https://YOUR-USERNAME.github.io/REPO-NAME/`
4. On your phone, open that URL in Safari/Chrome and tap **Add to Home Screen** — it behaves like a native app

> The repo being public is fine: it contains only code. Your expense data is never uploaded here.

#### 2. Import your e-invoices

Prerequisite: you have a carrier barcode (手機條碼) and have registered it (歸戶) on the MOF platform.

1. Log in to the [MOF E-Invoice Platform](https://www.einvoice.nat.gov.tw/)
2. Open your carrier consumption records, pick a month, and **export as CSV**
3. In the app, go to **Import Invoices** and drop the file in

Do this once at the start of each month and your entire month is booked in about a minute. Re-importing the same file is always safe.

#### 3. Set up cross-device sync (optional)

1. Create a second repo, e.g. `expense-data`, and set it to **Private** — this is where your data lives
2. GitHub → Settings → Developer settings → **Fine-grained personal access tokens** → generate a token:
   - Repository access: **Only select repositories** → tick only `expense-data`
   - Permissions → Contents: **Read and write**
3. In the app, open **Settings → GitHub Cloud Sync**, enter `your-username/expense-data` and the token, then press **Sync now**
4. Repeat the two fields once on each device; from then on, one tap merges records from both sides

> The token is stored only in each device's browser, and it is scoped to that single private repo — even if leaked, it cannot touch anything else on your account.

### 🔒 Privacy

- Expense data is stored in your browser's local storage and is **never** sent to any server
- Sync uses **your own** private GitHub repo, accessible only to your account
- The exchange-rate lookup requests only "current USD rate" and contains no personal data
- The public repo hosts only code; do **not** commit your invoice CSVs or JSON backups to it

### ⚠️ Notes

- Data is tied to the browser: clearing browser data deletes your records, so use the JSON backup or GitHub sync regularly
- USD conversion uses free mid-market rates (refreshed every 12 hours, last-known rate used offline); your credit card's actual rate will differ by roughly 1–2%, so treat converted figures as close estimates
- The CSV parser matches the platform's current (2026) export format; if the MOF changes it and imports break, please open an issue

---

*Built with vanilla HTML/JS + Chart.js. No build step, no dependencies to install — fork it and make it yours.*
