# Automatic Control (I) LLM Wiki — 自動控制 (I) 數位知識庫與互動式學習網站

本專案為依據 **Andrej Karpathy LLM Wiki 架構** 與 **Harness Engineering (`AGENTS.md` / `CLAUDE.md`)** 所建置的《自動控制 (I)》結構化知識庫與互動式網頁應用程式。

- **作者**：國立中正大學 機械工程學系四年級（中正機械四）廖紹強（412422092）
- **教材來源**：國立中正大學機械系 陳世樂教授《Automatic Control (I)》課程講義（Ch.1 ~ Ch.7，共 47 頁）

## 🚀 快速開始（如何上傳至 GitHub 並開啟 GitHub Pages）

1. 在 GitHub 建立一個新的公開儲存庫（Repository），例如命名為 `auto-control-wiki`。
2. 將本資料夾內的所有檔案（包含 `index.html`、`AGENTS.md`、`CLAUDE.md`、`GEMINI.md`、`README.md`、`SUBMISSION.md`、`raw/`、`wiki/`）上傳至該儲存庫根目錄。
3. 進入 GitHub Repository 的 **Settings** → 左側選單點選 **Pages**。
4. 在 **Build and deployment** 下方的 **Branch** 選擇 `main`（或 `master`），資料夾選擇 `/ (root)`，點擊 **Save**。
5. 等待約 1 分鐘後，即可透過 `https://<你的GitHub帳號>.github.io/auto-control-wiki/` 瀏覽互動式自動控制知識庫網站！

## 📂 專案目錄結構

- `AGENTS.md` / `CLAUDE.md` / `GEMINI.md`：定義知識庫結構、頁面格式與 AI 維護工作流（Ingest / Query / Lint / Web-Sync）的規則檔。
- `index.html`：單一檔案自包含互動式網站（內建 Wiki 閱讀器、Obsidian 雙向知識圖譜、二階步階響應與極點模擬器、波德圖模擬器、Routh 表計算機、規則與日誌檢視器）。
- `raw/`：存放原始不可變教材（`Automatic_Control_I_2026.pdf` 與 `llm-wiki-pattern.md`）。
- `wiki/`：由 AI 維護的結構化 Markdown 知識庫：
  - `wiki/index.md`：知識庫總目錄索引。
  - `wiki/log.md`：時間軸維護日誌。
  - `wiki/chapters/`：Chapter 1 至 Chapter 7 完整章節筆記（7 篇）。
  - `wiki/concepts/`：跨章節核心概念卡片（7 篇）。
- `SUBMISSION.md`：已整理好的作業繳交文案（含標題、專案簡介、規則延伸說明）。
