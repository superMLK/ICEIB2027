# NutriAgent — IEEE Conference Paper (LaTeX 練習)

這是「物聯網應用與資料分析」課程中練習 LaTeX 的專案。論文內容改寫自期末專題 NutriAgent 的提案書，用 ICEIB 2027 的 IEEE 雙欄研討會格式（`IEEEtran` conference）排成 6 頁的英文論文。

> **NutriAgent: A Grounded Conversational LLM Agent for Real-Time Dietary Decisions Integrating Wearable IoT and Clinical Nutrition Rules**
> Wei-Sheng Li, Huan Chen — Department of Computer Science and Engineering, National Chung Hsing University

## 論文定位

- **系統設計論文**：完整描述 NutriAgent 的設計，實驗章節寫實驗設計。
- **只放真實數字**：結果只有原型的離線功能檢核（5 組合成檔案、45 次評估）。預期成果都寫成 expected outcomes 或 hypotheses，沒有編造任何實驗數據。
- **揭露實作狀態**：Table III 列出哪些部分已實作、哪些還在規劃中。例如 Gemini 回覆的數值驗證目前還沒實作。
- 這是練習作品，**不是**實際投稿版本。

## 檔案結構

```
.
├── main.tex                    # 主檔：套件、TikZ 共用樣式、標題與作者，並組合各章節
├── references.bib              # 32 篇參考文獻（BibTeX）
├── sections/
│   ├── 00_abstract.tex         # 摘要與關鍵字（A–F）
│   ├── 01_introduction.tex     # I. Introduction（G–L），Fig. 1 定義在這裡
│   ├── 02_related_work.tex     # II. Related Work（M–O）
│   ├── 03_proposed_scheme.tex  # III. Proposed Scheme（PA–PM），Algorithm 1、2
│   ├── 04_experiments.tex      # IV. Experiments and Discussion（Q–V）
│   └── 05_conclusion.tex       # V. Conclusion（W–Z）
├── figures/
│   ├── architecture.tex        # Fig. 1 系統架構圖（TikZ，跨雙欄）
│   └── flowchart.tex           # Fig. 2 處理流程圖（TikZ，單欄）
├── tables/
│   ├── summary.tex             # Table I  Article Summary Table（12 篇 × 6 欄，跨雙欄）
│   ├── metrics.tex             # Table II 評估指標與目標
│   └── prototype.tex           # Table III 原型檢核結果與實作狀態
├── openspec/                   # OpenSpec 規劃文件（proposal、specs、design、tasks）
├── .olignore                   # 不上傳到 Overleaf 的檔案
└── README.md
```

括號裡的 A–Z 對應課程的 A2Z 論文寫作 SOP。

## 論文裡的 LaTeX 元素

| 元素 | 位置 | 練到的技巧 |
| --- | --- | --- |
| TikZ 架構圖 | `figures/architecture.tex` | 絕對座標、`-|`/`|-` 折線、虛線樣式、圖例、`figure*` 跨欄 |
| TikZ 流程圖 | `figures/flowchart.tex` | `positioning` 相對定位、菱形判斷框、分支與迴圈 |
| 演算法 | `sections/03_proposed_scheme.tex` | `algorithm` + `algpseudocode`（`noend`） |
| 公式 | `sections/03_proposed_scheme.tex` | `align`、`cases`、`\eqref` 交叉引用 |
| 表格 | `tables/` | `booktabs` 三線表、`table*` 跨欄、`p{}` 欄寬 |
| 參考文獻 | `references.bib` | `IEEEtran` 樣式、`@misc`/`@mastersthesis`、中文出處加註 `(in Chinese)` |

兩張圖共用的顏色與節點樣式（`na/...`）定義在 `main.tex`。顏色的意義：藍色是輸入輸出、紫色是 Planner、綠色是資料與分析、紅色是安全閘門、橘色是 LLM。

## 編譯

- 編譯器：**pdfLaTeX**（Overleaf 預設值）
- 主文件：`main.tex`
- 不需要任何外部圖檔，圖都是 TikZ 直接畫的。
- 目前的結果：**6 頁**，log 裡沒有錯誤、未定義引用或 Overfull 警告。

## 版本管理

論文在 Overleaf 上編譯，原始檔用這個 GitHub repo 管理。commit 時只加入論文相關的檔案：

```bash
git add main.tex references.bib sections figures tables
git commit -m "..."
git push
```

`myRef/`（專題參考資料）和 `.claude/` 已用 `.gitignore` 排除，不會推上 GitHub。

## 研討會資訊（參考）

論文格式參考 ICEIB 2027。以下資訊整理自官網，實際投稿前請再確認。

| 項目 | 內容 |
| --- | --- |
| 研討會 | 7th ICEIB 2027，淡江大學，2027 年 4 月 23–25 日 |
| 語言、頁數 | 英文，全文 4–8 頁 |
| 截止日 | 摘要 2027/3/1，全文 2027/4/20 |
| 出版 | IEEE 或 Engineering Proceedings |
