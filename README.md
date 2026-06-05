# 4C 學習成效即時評量平台 — 功能說明（中英對照）
# 4C Learning-Outcome Real-time Assessment Platform — Feature Guide (Bilingual)

> 版本 / Version：**v2.3**　·　單檔純前端 ＋ 選用雲端收集 / Single-file front-end + optional cloud collection
> 適用課程 / Course：專題系統（NPUST 資訊管理系）/ Project Systems (Dept. of Information Management, NPUST)

---

## 1. 平台簡介 / Overview

**中文**　以 4C 核心能力（批判思考、溝通協調、團隊合作、創新創造）為主軸，橫跨「心智圖 → IPO → Python → 應用模擬」四步驟教學，量測學生學習成效並評估教學設計。學生作答即時寫入網頁前端資料區，統計圖表與成效報告同步生成；可選擇連接 Google Apps Script 後端，達成跨裝置雲端收集。

**English**　Built around the 4C competencies (Critical Thinking, Communication, Collaboration, Creativity) across a four-step teaching flow (Mind Map → IPO → Python → Applied Simulation), this platform measures student learning outcomes and evaluates teaching effectiveness. Responses are written instantly to the in-page front-end data store, with charts and reports generated in real time. An optional Google Apps Script backend enables cross-device cloud collection.

---

## 2. 評量架構 / Assessment Framework

**4C × 三技術面向矩陣（12 題）＋ 教學成效（4 題）＝ 16 題，李克特 5 點量表**
**4C × three technical dimensions (12 items) + teaching effectiveness (4 items) = 16 items, 5-point Likert**

| 面向 / Dimension | 批判思考 / Critical | 溝通協調 / Communication | 團隊合作 / Collaboration | 創新創造 / Creativity |
|---|---|---|---|---|
| 心智圖 / Mind Map | ✔ | ✔ | ✔ | ✔ |
| IPO 分析 / IPO | ✔ | ✔ | ✔ | ✔ |
| Python 編碼 / Python | ✔ | ✔ | ✔ | ✔ |

教學成效題（T1–T4）/ Teaching-effectiveness items:
1. 四步驟教學（心智圖、IPO、Python、應用模擬）有助於我的學習 / The four-step teaching aids my learning
2. 教師的講解與示範清楚易懂 / The instructor's explanations and demos are clear
3. 課程提升了我發現與解決問題的能力 / The course improved my problem-finding/solving
4. 我願意推薦本課程 / I would recommend this course

---

## 3. 功能總覽 / Feature Overview

| # | 分頁 / Tab | 中文說明 | English Description |
|---|---|---|---|
| ① | 總覽 / Home | 平台介紹、即時 KPI、AI 賦能學習生態、運作流程 | Intro, live KPIs, AI-empowered ecosystem, workflow |
| ② | 學生作答 / Answer | 16 題自評；連續作答（Kiosk）模式 | 16-item self-assessment; continuous (kiosk) mode |
| ③ | 前端資料區 / Data | 即時資料庫、快速批次輸入、匯入匯出、後端下載 | Live data store, quick batch entry, import/export, backend download |
| ④ | 即時統計 / Stats | KPI、趨勢、熱力圖、雷達、信度、分組比較 | KPIs, trends, heatmap, radar, reliability, group comparison |
| ⑤ | 報告與建議 / Report | 成效等第、強弱項、教學改善建議、列印 PDF | Grade, strengths/weaknesses, teaching advice, print PDF |
| ⑥ | 學習導覽 / Guide | 課程平台、教學頻道、各面向推薦工具 | Course platforms, teaching channel, recommended tools |

---

## 4. 資料蒐集模式 / Data Collection Modes

| 模式 / Mode | 中文 | English |
|---|---|---|
| 本機 / Local | 留空 `COLLECT_URL`；作答存於瀏覽器 localStorage，適合單機／Kiosk | Leave `COLLECT_URL` empty; data in browser localStorage; ideal for single-device/kiosk |
| 雲端 / Cloud | 設定 `COLLECT_URL`；各裝置作答 POST 至 Google 試算表，統計端 GET 同步 | Set `COLLECT_URL`; devices POST to a Google Sheet, stats GET-sync |

加速蒐集 / Faster collection：**連續作答模式**（送出即重置）與**快速批次輸入**（一列鍵入紙本、Enter 即存）。
**Continuous mode** (auto-reset after submit) and **quick batch entry** (type a paper row, press Enter).

---

## 5. 統計與分析 / Statistics & Analytics

| 項目 / Item | 中文 | English |
|---|---|---|
| 關鍵指標 / KPIs | 作答數、整體 4C、教學成效、信度 | Count, overall 4C, teaching, reliability |
| 即時趨勢 / Trend | 累積作答數、四線累積平均、分時段趨勢表 | Cumulative count, 4-line running averages, segmented table |
| 熱力圖 / Heatmap | 4C × 三面向各格平均（紅→綠） | 4C × dimension cell means (red→green) |
| 雷達圖 / Radar | 4C 能力輪廓（純 SVG） | 4C profile (pure SVG) |
| 信度 / Reliability | 12 題 Cronbach α | Cronbach's α over 12 items |
| 分組比較 / Group compare | 資訊相關 vs 非資訊相關 成效對照 | IT-related vs non-IT outcome comparison |

學生分類 / Student classification：**資訊相關 / 非資訊相關 / 未分類**（作答時選填）。
**IT-related / Non-IT / Unclassified** (selected at answering time).

---

## 6. 雲端後端結構 / Cloud Backend Structure

Google Apps Script Web App，建立試算表「4C評量收集區」/ creates a Sheet "4C評量收集區":

| 工作表 / Worksheet | 內容 | 欄數 / Columns |
|---|---|---|
| 收集區 / Master | 全部作答，含「資訊背景」欄 / all responses, with classification column | 20（含 bg）|
| 資訊相關 / IT-related | 該分類鏡射 / mirror of that group | 19（無 bg）|
| 非資訊相關 / Non-IT | 同上 / same | 19 |
| 未分類 / Unclassified | 未選分類 / no class chosen | 19 |

- `doPost`：**雙寫**——總表（含 bg）＋對應分類分頁（無 bg）/ dual-write to master + group sheet
- `doGet`：**只讀總表**，避免重複計算 / reads master only to avoid double counting
- `setup()`：授權並建立工作表 / authorize & create sheets
- `reset()`：清空舊資料並重建對齊標頭（解決欄位錯位）/ clear old data & rebuild aligned headers
- 標頭自動對齊 / headers auto-aligned on each write

---

## 7. 部署步驟 / Deployment Steps

**中文**
1. 於「前端資料區 → ☁ 雲端收集區後端」下載 `.gs`。
2. 貼入 `script.google.com` 新專案；**先執行一次 `setup()`**（首次需點「進階 → 前往 → 允許」授權）。
3. 部署 → 新增部署作業 → **網頁應用程式**；執行身分＝**我**；誰可以存取＝**任何人**。
4. 複製 `/exec` 網址，貼到本檔頂端 `COLLECT_URL`。
5. 改後端程式後，需「管理部署作業 → 編輯 → 版本：**新版本**」重新部署。
6. 若欄位錯位（混到舊資料），執行一次 **`reset()`**。

**English**
1. Download the `.gs` from "Data → Cloud Backend".
2. Paste into a new `script.google.com` project; **run `setup()` once** (first run: Advanced → Go → Allow).
3. Deploy → New deployment → **Web app**; Execute as **Me**; Access **Anyone**.
4. Copy the `/exec` URL into `COLLECT_URL` at the top of this file.
5. After editing the backend, redeploy via Manage deployments → Edit → **New version**.
6. If columns are misaligned (mixed old data), run **`reset()`** once.

---

## 8. 設定項（檔案頂端可編輯）/ Configuration (editable at top)

| 變數 / Variable | 中文 | English |
|---|---|---|
| `COLLECT_URL` | 雲端收集區網址；留空＝本機模式 | Cloud endpoint; empty = local mode |
| `AUTO_REFRESH_SEC` | 雲端自動同步秒數（0＝關閉）| Cloud auto-sync seconds (0 = off) |
| `COURSE_PLATFORMS` | 課程平台名稱／網址／簡介 | Platform name/url/description |
| `CHANNEL_URL` | YouTube 教學頻道網址 | YouTube teaching channel URL |
| `DIM_VIDEO` | 各面向教學影片（單一影片可內嵌）| Per-dimension videos (single videos embeddable) |
| `BG_OPTS` | 資訊背景分類選項 | IT-background classification options |
| `DB_KEY` | 本機資料庫鍵名 | localStorage key |

> 註 / Note：YouTube **頻道**網址無法內嵌播放，僅以連結卡呈現；單一影片才會內嵌。
> A YouTube **channel** URL cannot be embedded (shown as a link card); only single videos embed.

---

## 9. 版本沿革 / Changelog

| 版本 / Ver. | 中文 | English |
|---|---|---|
| 2.0 | 純前端重建：作答→前端資料區→即時統計 | Front-end rebuild: answer → data store → live stats |
| 2.1 | 接回雲端收集（Apps Script Web App）＋即時趨勢 | Cloud collection (Apps Script) + live trends |
| 2.2 | T1 改四步驟教學；新增資訊背景分類與分組比較 | Four-step T1; IT-background classification & group comparison |
| 2.3 | 後端總表＋分類分頁雙寫；`reset()`、標頭自動對齊；修正欄位錯位；AI 賦能 UI、平台／頻道連結；修正 YouTube 嵌入錯誤 153 | Master + group dual-write; `reset()`, header auto-align; fixed misalignment; AI-empowered UI, platform/channel links; fixed YouTube error 153 |

---

## 10. 注意事項 / Notes & Limitations

- **本機模式各裝置資料獨立** / Local-mode data is per-device; 跨裝置集中收集需設定雲端後端 / use cloud backend for centralized cross-device collection.
- **隱私 / Privacy**：本機資料僅存於瀏覽器、不外傳；雲端資料存於教師的 Google 試算表 / local data stays in the browser; cloud data lives in the teacher's Google Sheet.
- **沙箱 / Sandbox**：若預覽環境禁用 localStorage，系統自動降級為本次階段暫存並提示匯出備份 / if localStorage is blocked, the app degrades to session-only and prompts for export.
- **備份 / Backup**：建議定期匯出 JSON／CSV / export JSON/CSV periodically.

---

*文件版本對應應用程式 v2.3 / This document corresponds to app v2.3.*
