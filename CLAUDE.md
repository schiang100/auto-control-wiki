# AGENTS.md — 自動控制 (I) LLM Wiki 知識庫與互動講義網站規範

> **專案名稱**：Automatic Control (I) LLM Wiki — 自動控制 (I) 數位知識庫與互動式學習網站  
> **作者**：國立中正大學 機械工程學系四年級（中正機械四）廖紹強（學號：412422092）  
> **知識來源**：國立中正大學機械工程學系 陳世樂教授《Automatic Control (I)》課程講義（共 7 章、47 頁）  
> **架構模式**：Andrej Karpathy LLM Wiki 三層知識庫架構（`raw/` + `wiki/` + `AGENTS.md`/`CLAUDE.md` Schema）

---

## 1. 專案目的（Project Purpose）

本專案旨在解決傳統 RAG（檢索增強生成）每次提問都必須重新拼湊碎片、知識無法累積的問題。透過 **LLM Wiki 模式**，讓 AI Agent 扮演「知識庫維護者（Wiki Maintainer）」，將《Automatic Control (I)》原始課程講義（`raw/`）逐步編譯、拆解並交叉連結為結構化的持久知識庫（`wiki/`），同時同步渲染至可公開瀏覽的 **互動式課程知識庫網頁（`index.html`）**。

本專案的核心目標：
1. **累積式個人知識庫**：將自動控制七大核心章節（緒論、數學模型、系統動態與暫態性能、穩定度分析、穩態誤差、根軌跡法、頻率響應法）轉化為具備雙向連結（`[[Wiki Links]]`）與 YAML Frontmatter 的結構化筆記。
2. **規則驅動開發（Harness Engineering）**：定義明確的目錄結構、命名規則、標準作業流程（Ingest / Query / Lint / Sync Web），使任何 AI Agent（Claude Code、OpenAI Codex、Gemini CLI / Antigravity、Cursor）讀取本檔案後，都能以完全一致的標準繼續新增章節、擴充概念條目或更新網頁內容。
3. **理論與互動視覺化結合**：在前端網頁整合 **Markdown 知識庫瀏覽器**、**互動式知識關聯圖譜（Knowledge Graph）** 與 **控制系統動態模擬器（一階/二階步階響應、Routh 表檢測、波德圖漸近線）**，達成可立即在 GitHub Pages 瀏覽與複習的學習成果。

---

## 2. 三層系統架構與目錄規範（Architecture & Directory Structure）

本專案嚴格遵循 LLM Wiki 三層架構，檔案命名與放置規則如下：

```text
auto-control-wiki/
├── AGENTS.md                  # [Schema 層] 通用 AI Agent 規則、工作流與開發規範（主設定檔）
├── CLAUDE.md                  # [Schema 層] Claude Code 專用規則設定檔（與 AGENTS.md 同步）
├── GEMINI.md                  # [Schema 層] Gemini / Antigravity 專用規則設定檔
├── index.html                 # [Web 展示層] GitHub Pages 主網頁（整合 Wiki 檢視、知識圖譜與互動模擬）
├── README.md                  # 專案說明、架構導覽與本地端/GitHub Pages 執行指南
├── SUBMISSION.md              # 課程作業繳交格式整理（含專案簡介、連結與規則延伸說明）
├── raw/                       # [Layer 1: Raw Sources 原始資料層]（唯讀、不可修改）
│   ├── Automatic_Control_I_2026.pdf   # 陳世樂教授《Automatic Control (I)》47頁完整講義
│   └── llm-wiki-pattern.md            # LLM Wiki 架構設計理念文件
└── wiki/                      # [Layer 2: The Wiki 知識庫層]（由 AI 全權建立與維護）
    ├── index.md               # 知識庫總目錄（依「章節總覽」與「核心概念」分類索引）
    ├── log.md                 # 時間軸維護日誌（Append-only，紀錄每次 ingest / query / lint）
    ├── chapters/              # 章節筆記目錄（命名格式：ch0X-kebab-case-name.md）
    │   ├── ch01-introduction.md
    │   ├── ch02-mathematical-model.md
    │   ├── ch03-system-dynamics-transient.md
    │   ├── ch04-stability.md
    │   ├── ch05-steady-state-error.md
    │   ├── ch06-root-locus.md
    │   └── ch07-frequency-response.md
    └── concepts/              # 跨章節核心概念卡片（命名格式：kebab-case-concept.md）
        ├── transfer-function-and-causality.md
        ├── second-order-performance-indices.md
        ├── effects-of-zeros-and-dominant-poles.md
        ├── routh-hurwitz-criterion.md
        ├── system-type-and-error-constants.md
        ├── root-locus-construction-rules.md
        └── bode-plot-and-stability-margins.md
```

### 檔案命名與放置鐵則：
1. **`raw/` 絕對唯讀原則**：所有新放入的教材 PDF、考古題、MATLAB 腳本或課堂筆記圖檔一律放入 `raw/`。AI **絕對不可修改或刪除** `raw/` 內的任何檔案。
2. **章節頁面命名**：一律放置於 `wiki/chapters/`，檔名採用 `ch{兩位數章節}-{英文小寫連字號}.md`（例如新增第 8 章狀態空間分析，應命名為 `wiki/chapters/ch08-state-space-analysis.md`）。
3. **概念頁面命名**：一律放置於 `wiki/concepts/`，檔名採用 `{英文小寫連字號}.md`（例如新增 PID 控制器調校，應命名為 `wiki/concepts/pid-controller-tuning.md`）。

---

## 3. 新增內容時必須遵守的格式（Content & Page Formatting Rules）

### 3.1 Wiki Markdown 頁面格式規範
每一篇位於 `wiki/chapters/` 或 `wiki/concepts/` 的 Markdown 檔案，**必須**包含以下結構：

1. **YAML Frontmatter（必備標頭）**：
   ```yaml
   ---
   id: ch03-system-dynamics-transient
   title: "Chapter 3: 系統動態與暫態性能 (System Dynamics and Transient Performance)"
   category: chapter          # 僅限 chapter | concept | comparison | example
   chapter: "Chapter 3"
   source_pages: "pp. 9–20"   # 明確標註對應 raw/Automatic_Control_I_2026.pdf 之頁碼
   updated: 2026-11-14
   tags: [一階系統, 二階系統, 阻尼比, 暫態性能指標, 超越量]
   related: [ch02-mathematical-model, second-order-performance-indices, effects-of-zeros-and-dominant-poles]
   ---
   ```
2. **頁面內容區塊順序**：
   - **`# 標題`**：清楚標示中英文主題名稱。
   - **`> 核心摘要（Executive Summary）`**：以 2–3 句話總結該頁在自動控制系統設計中的物理意義與定位。
   - **`## 核心理論與數學推導`**：完整呈現講義推導過程，所有數學公式必須使用標準 LaTeX 語法（行內公式 `$G(s) = \frac{N(s)}{D(s)}$`，獨立公式區塊 `$$ ... $$`），並保留講義公式編號（如 `Eq. (3-24)`）。
   - **`## 物理意義與工程直覺`**：用機械與控制工程語言解釋公式背後的物理現象（如極點實部決定指數衰減包絡線、虛部決定振盪頻率）。
   - **`## 典型例題與習題解析`**：收錄講義中的 `Example` 與 `Exercise`（如進給驅動系統、溫度計液柱、RLC 電路、標準二階步階響應判讀）。
   - **`## 關聯知識節點（Cross-References）`**：使用雙向連結語法 `[[page-id|顯示名稱]]` 串聯前後章節與核心概念。

### 3.2 `wiki/index.md` 與 `wiki/log.md` 維護格式
- **`wiki/index.md`**：為內容導向的總目錄，分為「章節講義筆記（Chapters）」與「核心觀念卡片（Concepts）」兩大表格，包含頁面連結、對應 PDF 頁碼、一句話摘要與標籤。每次新增或修改頁面時**必須同步更新**。
- **`wiki/log.md`**：為時間軸導向的唯讀追加紀錄（Append-only）。每筆紀錄開頭必須嚴格採用格式：
  ```markdown
  ## [YYYY-MM-DD] <operation> | <Title or Summary>
  - **影響檔案**：列出新增或更新的檔案路徑
  - **變更摘要**：說明本次萃取、查詢或健檢的重點
  ```
  其中 `<operation>` 僅限 `init`、`ingest`、`query`、`lint`、`web-sync`。此格式確保可用 `grep "^## \[" wiki/log.md | tail -5` 快速解析最近 5 筆操作。

---

## 4. 網頁的整體風格與頁面結構（Web Style & UI Structure）

主網頁 `index.html` 為單一自包含（Self-contained）互動式知識庫網站，設計風格與結構規範如下：

1. **整體視覺風格（Academic Engineering Dark & Clean Tech）**：
   - 採用深色科技導航列搭配高對比清晰閱讀區（支援深淺主題切換），主色調為 **工程靛藍（Indigo `#4f46e5`）**、**控制系統青（Cyan `#06b6d4`）** 與 **穩定度翠綠（Emerald `#10b981`）**。
   - 數學公式透過 KaTeX 渲染為出版級排版；方塊圖與圖譜採用高解析 SVG 繪製。
2. **四大核心頁面結構（Navigation Tabs）**：
   - **📚 知識庫閱讀器（Wiki Reader）**：左側為章節與概念分類側邊欄 + 即時關鍵字搜尋框；中央為筆記本文區（顯示 YAML 標籤、對應講義頁碼、LaTeX 公式、例題解析與底部雙向關聯卡片）；點擊任何關聯卡片可無縫跳轉至對應條目。
   - **🕸️ 知識關聯圖譜（Obsidian Graph View）**：以互動式節點網路圖呈現 7 大章節與 7 大核心概念之間的雙向連結關係，點擊節點即可預覽摘要並開啟該篇筆記。
   - **🎛️ 控制系統互動實驗室（Interactive Control Lab）**：
     - **二階系統步階響應與極點分佈模擬器**：即時調整阻尼比 $\zeta$ 與自然頻率 $\omega_n$，動態繪製 $y(t)$ 步階響應曲線、$s$ 平面極點位置（LHP），並自動計算上升時間 $T_r$、峰值時間 $T_p$、2% 安定時間 $T_s$ 與最大超越量 $\%OS$。
     - **波德圖（Bode Plot）與頻域穩定度模擬器**：動態呈現增益圖（dB）與相位圖（deg）及轉折頻率特性。
     - **羅斯-赫維茲（Routh-Hurwitz）穩定度檢驗器**：輸入特徵多項式係數（內建講義範例與習題預設按鈕），自動生成 Routh 表並判讀右半平面（RHP）極點數。
   - **🤖 Agent 規則與維護日誌（Schema & Log Viewer）**：直接在網頁上檢視 `AGENTS.md` 開發規範、`wiki/index.md` 總索引表與 `wiki/log.md` 演進紀錄。

---

## 5. AI 修改或新增內容時需要遵守的操作工作流（AI Operational Workflows）

當使用者對 AI Agent 下達指令時，請依照以下四大標準流程執行：

### 流程 A：`Ingest`（消化新教材或新章節）
1. **讀取原始檔**：從 `raw/` 讀取新增的教材、講義頁碼或補充筆記（絕不修改 `raw/` 原檔）。
2. **更新或建立 Wiki 頁面**：
   - 若為新章節，於 `wiki/chapters/` 建立 `chXX-*.md`；若為深入專題（如 Lead-Lag 補償器、Nyquist 圍繞判據），於 `wiki/concepts/` 建立概念卡。
   - 在既有相關頁面的 `related` 欄位與文末「關聯知識節點」加入雙向連結，若有新資訊修正或補充舊頁面，直接更新舊頁面並註記來源頁碼。
3. **更新索引與日誌**：在 `wiki/index.md` 新增條目，並在 `wiki/log.md` 附加一筆 `## [YYYY-MM-DD] ingest | ...` 紀錄。
4. **同步網頁資料**：將新頁面的結構化資料同步寫入 `index.html` 內的 `WIKI_PAGES` 陣列與知識圖譜邊集（Edges），確保網頁即時呈現最新知識庫。

### 流程 B：`Query`（知識庫問答與知識回存）
1. **優先查閱索引**：先讀取 `wiki/index.md` 鎖定相關章節與概念頁，再深入閱讀對應的 `.md` 檔案。
2. **附帶明確引用回答**：回答時必須標註知識庫頁面與原始講義頁碼（例如：`參見 [[ch04-stability]] 與講義 pp. 24–26`）。
3. **高價值解答回存（File Back to Wiki）**：若使用者的提問產生了跨章節綜合比較（如「時域性能指標 vs. 頻域性能指標對照表」或新習題詳解），應主動將該解答整理為新的概念頁存入 `wiki/concepts/`，並更新 `index.md`、`log.md` 與 `index.html`。

### 流程 C：`Lint`（知識庫健康度檢查）
定期執行以下檢查並記錄於 `wiki/log.md`：
1. **孤兒頁面檢查（Orphan Check）**：確保每篇 `wiki/` 頁面至少有 2 個雙向關聯連結，無孤立節點。
2. **符號與公式一致性檢查**：確保全庫統一使用講義符號（如轉移函數 $G(s)$、閉迴路轉移函數 $T(s)$、阻尼比 $\zeta$、自然頻率 $\omega_n$、誤差常數 $K_p, K_v, K_a$）。
3. **網頁同步檢查**：確認 `wiki/` 目錄下的所有 Markdown 頁面皆已完整映射至 `index.html` 的前端知識庫中。
