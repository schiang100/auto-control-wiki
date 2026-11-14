# 作業繳交內容整理（可直接複製貼上至作業繳交區）

## 📌 繳交標題
`中正 412422092 機械四 廖紹強 自動控制(I) LLM Wiki 知識庫與互動學習網站`

---

## 1. 專案簡介
本專案結合 Andrej Karpathy 提出的 **LLM Wiki 三層知識庫架構（`raw/` 原始教材層 + `wiki/` 結構化雙向連結知識庫層 + `AGENTS.md`/`CLAUDE.md` 規則層）**，以國立中正大學機械工程學系陳世樂教授《Automatic Control (I)》課程講義（共 7 章、47 頁）為核心知識來源，打造一個可持續由 AI Agent 自動維護與擴充的**「自動控制 (I) 數位知識庫與互動式學習網站」**。

專案成果包含：
1. **完整的 LLM Wiki 知識庫（`wiki/`）**：涵蓋 7 大章節完整講義筆記（`wiki/chapters/ch01` ~ `ch07`）、7 張跨章節核心概念卡片（`wiki/concepts/`，包含轉移函數因果性、二階暫態性能指標、零點下衝與主導極點、羅斯-赫維茲特殊情況、系統型別與時頻域誤差常數、根軌跡九大法則、波德圖與穩定邊限），以及自動維護的總索引 `wiki/index.md` 與時間軸日誌 `wiki/log.md`。
2. **互動式學習網站（`index.html`）**：將 LLM Wiki 完整呈現在網頁上，內建 **Markdown 知識庫閱讀器（含 KaTeX 數學公式渲染與雙向連結跳轉）**、**Obsidian 風格互動式知識關聯圖譜（Knowledge Graph）**、**控制系統互動模擬實驗室（即時調整 $\zeta, \omega_n$ 觀察二階步階響應與極點移動、波德圖漸近線模擬器、Routh-Hurwitz 穩定度自動檢驗器）**，以及 **`AGENTS.md` / `index.md` / `log.md` 線上檢視器**。

---

## 2. GitHub Repository 連結
`https://github.com/<你的GitHub帳號>/auto-control-wiki`  
*(上傳後請將 `<你的GitHub帳號>` 替換為你的實際 GitHub 帳號名稱)*

---

## 3. 網頁連結（GitHub Pages）
`https://<你的GitHub帳號>.github.io/auto-control-wiki/`  
*(開啟 GitHub Pages 後請替換為你的實際網址)*

---

## 4. `AGENTS.md` / `CLAUDE.md`
完整設定檔已存放於專案根目錄之 [`AGENTS.md`](./AGENTS.md) 與 [`CLAUDE.md`](./CLAUDE.md)（網頁上方切換至「🤖 Agent 規則與維護日誌」分頁亦可直接瀏覽全文）。

---

## 5. 簡短說明：如果之後要新增一個新的頁面或內容，你設計的規則可以如何幫助 AI 繼續完成？

在 `AGENTS.md`（與 `CLAUDE.md`）中，我設計了一套結構化的 **Harness Engineering 規則與四大標準作業流程（Ingest / Query / Lint / Web-Sync）**。當未來我要新增新的學習內容（例如放入《Automatic Control (II)》的「Chapter 8: 狀態空間分析 State-Space Analysis」、新的考古題 PDF，或新增「超前-落後補償器（Lead-Lag Compensator）」概念頁）時，AI 只要讀取 `AGENTS.md`，就能自動且一致地完成以下五個步驟，無須每次重新口頭交代格式：

1. **嚴格的檔案分類與命名規範（Directory & Naming Rules）**：
   AI 知道原始教材必須放入唯讀的 `raw/` 目錄不可竄改；若新增章節則依 `wiki/chapters/ch08-state-space-analysis.md` 規則命名，若新增跨章節專題則依 `wiki/concepts/lead-lag-compensator.md` 命名，確保專案結構永遠整潔一致。
2. **統一的頁面模版與公式標準（YAML Frontmatter & Content Schema）**：
   AI 新增任何 Markdown 筆記時，會自動套用規定的 YAML 標頭（標註 `id`、`category`、`chapter`、`source_pages` 對應頁碼、`tags` 與 `related` 關聯節點），並依序撰寫「核心摘要 → 數學推導（保留講義公式編號與 LaTeX 語法）→ 物理意義與工程直覺 → 例題解析 → 雙向連結」。
3. **自動交叉引用與既有頁面更新（Cross-Referencing）**：
   根據 `Ingest` 流程，AI 新增一篇筆記時，不會只產生孤立檔案，而會主動檢索並更新既有相關頁面（例如新增「狀態空間可控性」時，自動在 `ch02-mathematical-model.md` 與 `ch04-stability.md` 的 `related` 區塊補上雙向連結 `[[ch08-state-space-analysis]]`）。
4. **自動維護索引與時間軸日誌（Indexing & Logging）**：
   AI 會在 `wiki/index.md` 的分類表格新增該頁的一句話摘要與頁碼索引，並在 `wiki/log.md` 尾端追加一筆標準格式 `## [YYYY-MM-DD] ingest | ...` 紀錄，讓知識庫具備完整的演進歷史與可檢索性。
5. **無縫同步至前端互動網頁（Web Synchronization）**：
   `AGENTS.md` 明確規範了 `index.html` 內 `WIKI_PAGES` 資料結構與知識圖譜節點格式，AI 在更新完 Markdown 知識庫後，能直接將新條目同步寫入 `index.html`，讓網頁上的側邊欄目錄、知識關聯圖譜（Graph View）與搜尋引擎立即顯示新內容，真正達成「規則一次定義，知識庫與網站持續自動演進」的目標。
