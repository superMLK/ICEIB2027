# Proposal

## Why

這個 repo 是課程中練習 LaTeX 的專案，目前只有一份空的 ICEIB 2027 IEEE 模板（佔位文字與 `example-image`）。要真正練到 IEEE 論文排版（雙欄浮動體、TikZ、演算法、公式、BibTeX），需要一份有實際內容的論文。期末專題 NutriAgent 的提案書（`myRef/NutriAgent_Proposal_第10組.docx`）內容完整，適合改寫成一篇 6 頁的系統設計論文。

寫作方向已在 grilling 訪談中逐項定案，這份 change 把那些決定固定下來，避免實作時偏離（例如編造實驗數字、版面超頁、引用錯誤文獻）。

## What Changes

- 把模板的佔位內容換成 NutriAgent 論文的英文內容，依 A2Z SOP 結構撰寫 Abstract、I–V 節。
- 內容唯一來源是 `.docx` 提案書；`NutriAgent 提案解析.pdf` 描述的是舊版（PyTorch Neural Contextual Bandit），不採用。
- 論文定位為系統設計論文：實驗章節寫實驗設計，結果只放原型離線檢核的真實數字，並標示為 preliminary / functional verification；預期成果寫成 hypotheses，**不編造任何數字**。
- 提案書第 4 章 Deep Research 拆散融入：5 個思維模式 → 第 III 節 Design Principles；3 個爭論 → 第 IV 節 Discussion；10 個深度問題只取 2 題改寫為範例情境。
- 4 段 pseudocode 只保留 2 段：Algorithm 1 = 臨床與過敏原閘門、Algorithm 2 = 混合排序與接地回覆；主流程改用流程圖，偏好更新改用公式。
- 用 TikZ 以英文重畫系統架構圖（跨雙欄 `figure*`）與處理流程圖（單欄），取代 docx 裡的中文 PNG。
- Related Work 以跨雙欄的 Article Summary Table 整理 12 篇核心文獻（取代原本的既有方法比較表），內容只取自 docx 3.3 節的彙整表。
- 32 篇參考文獻轉成 BibTeX；其中 9 篇（[2][9][13][16][17][18][19][30][32]）上網核對，中文出處翻譯標題並加註 `(in Chinese)`。
- 寫出完整設計，並在實驗章節附 Implementation Status，標明哪些已實作、哪些尚在規劃。
- 標題與作者改為 NutriAgent 論文的實際資訊；作者 Wei-Sheng Li、Huan Chen，email 用佔位。
- **BREAKING（對模板而言）**：移除模板的示範內容與 `tables/comparison.tex` 佔位表。

## Capabilities

### New Capabilities
- `paper-content`：論文的文字內容、章節結構（A2Z SOP 對應）、資料誠實性（不編造數字、實作狀態揭露）與作者資訊。
- `paper-figures`：TikZ 重畫的架構圖與流程圖、2 段演算法、表格與公式的呈現要求。
- `paper-references`：BibTeX 文獻庫的轉換、核對與中文出處處理規則。
- `paper-build`：透過 olcli 同步並在 Overleaf 上驗證編譯、6 頁篇幅限制與 GitHub 發布流程。

### Modified Capabilities
（無，`openspec/specs/` 目前沒有既有 capability。）

## Impact

- **修改的檔案**：`main.tex`、`references.bib`、`sections/00_abstract.tex`–`05_conclusion.tex`。
- **新增的檔案**：`figures/architecture.tex`、`figures/flowchart.tex`、`tables/` 下的實際表格檔；刪除 `tables/comparison.tex`。
- **相依套件**：`main.tex` 新增 `tikz`（及其 library），其餘沿用模板套件；編譯器維持 pdfLaTeX。
- **外部系統**：Overleaf 專案 AIoT_DIC4 透過 olcli 同步（`olcli push`）與編譯（`olcli pdf`、`olcli output log`），以 `.olignore` 排除 `myRef/`、`openspec/`、`.claude/`、`README.md`、`overleafInstruction.md`。GitHub 公開 repo `superMLK/ICEIB2027` 只在完成後 push，且 push 前需詢問使用者。
- **開發工具**：曾嘗試 Overleaf Git 整合（remote `overleaf`、user-scope MCP `overleaf-AIoT_DIC4`），但 Overleaf 回應 `no git access`，兩者目前未使用。
- **不受影響**：`myRef/`（由 `.gitignore` 與 `.olignore` 排除，只讀取不修改）、`README.md`。
