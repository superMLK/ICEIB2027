# Design

## Context

- 模板已是模組化結構：`main.tex` 只負責 `\input` 各節，內容在 `sections/00–05`、`tables/`、`figures/`。已用 pdfLaTeX 在 Overleaf 編譯成功（3 頁佔位內容）。
- 本機沒有 LaTeX，**唯一的編譯環境是 Overleaf**，透過 olcli 操作：`olcli push` 同步、`olcli pdf` 編譯並下載 PDF、`olcli output log` 取得 log，因此我可以自己檢查錯誤與頁數。
- 曾嘗試改用 Overleaf Git 整合（remote `overleaf`、user-scope MCP `overleaf-AIoT_DIC4`），Token 驗證成功，但 Overleaf 回應 `no git access`，因此改回 olcli。該 remote 與 MCP 目前未使用。
- 內容來源 docx 是中文，約 700 行文字，份量遠超 6 頁，需要大量取捨（取捨原則見 proposal 與 `specs/paper-content`）。

## Goals / Non-Goals

**Goals:**
- 邊寫邊編譯，讓 TikZ 與浮動體的錯誤在小範圍內被發現。
- 把篇幅控制在 6 頁，並保留調整的空間。

**Non-Goals:**
- 不修改 NutriAgent 的系統設計本身；論文只呈現 docx 的設計。
- 不重寫 `README.md`。
- 不處理 ICEIB 實際投稿要求（這是練習專案）。

## Decisions

### D1. 各節篇幅預算（6 頁 = 12 欄）

| 區塊 | 預算（欄） | 主要內容 |
| --- | --- | --- |
| 標題、作者、Abstract | 0.6 | — |
| I. Introduction | 1.6 | 動機、挑戰、貢獻、章節安排 |
| II. Related Work | 約 1.85 | 4 個子題（精簡）+ 跨雙欄 Article Summary Table |
| III. Proposed Scheme | 4.6 | 問題定義、Design Principles、Fig. 1（跨欄，約 0.9 欄 × 2）、能量預算、Algorithm 1、排序與接地回覆、Algorithm 2、偏好更新、Fig. 2 |
| IV. Experiments | 2.4 | 資料集、baseline、指標表、原型結果與實作狀態表、Discussion |
| V. Conclusion | 0.4 | — |
| References | 0.8 | 約 25–32 筆引用 |

Article Summary Table 比原本的既有方法比較表多出約 0.25 欄，以刪減 II 節逐篇介紹的文字抵銷（表格已列出各篇的方法、發現與限制，內文不必重複）。

超出時的刪減順序：先壓縮 Related Work 的文字 → 精簡流程圖節點 → 合併指標表與結果表 → 縮減 Discussion。不刪 Algorithm 1/2、Fig. 1 與 Article Summary Table。

### D2. 架構圖用橫向佈局的 `figure*`
原圖是直式長圖，縮到單欄會看不清楚。改成橫向三欄佈局：左邊外部來源，中間 8 個模組排成兩列（①–④ 上列、⑤–⑧ 下列，以 S 形資料流連接），右邊 K1–K4。放在第 2 頁頂端（`[t]`），因為 `figure*` 只能出現在頁面頂端，而且最早只能出現在定義所在頁的下一頁。
- 替代方案：單欄直式圖 → 字太小，且一張圖占掉大半欄，所以不採用。

### D3. 流程圖精簡為單欄
保留 spec 要求的 6 個判斷點，把相鄰的處理節點合併（例如「合理性檢查 → 動態 TDEE → 當餐預算」合成一個節點）。使用 TikZ 的 `positioning` 與 `arrows.meta`，主幹垂直排列，例外分支向右、回饋迴圈向左。字級用 `\footnotesize`，`\resizebox` 只作為最後手段（會讓字級和內文不一致）。

### D4. TikZ 圖放在獨立檔案
`figures/architecture.tex` 與 `figures/flowchart.tex` 只包含 `tikzpicture`，由章節檔以 `\input` 引入，外層的 `figure` 環境留在章節檔。共用樣式（顏色、節點形狀）定義在 `main.tex` 的 preamble，讓兩張圖風格一致。顏色沿用原圖的語意：藍色為輸入輸出、紫色為 Planner、綠色為資料與分析、紅色為安全閘門、橘色為 LLM。

### D5. 演算法套件
沿用模板的 `algorithm` + `algpseudocode`。由於 IEEE 雙欄寬度有限，長行用 `\State` 搭配手動斷行或縮寫變數名稱，不使用跨欄的 `algorithm*`。

### D6. 文獻核對方式
用 WebSearch 查 arXiv ID、DOI 或官方網址，只核對 9 篇。核對結果先寫入 bib，有問題的條目在 bib 中加註 `% UNVERIFIED` 並暫不引用，彙整後一次回報使用者。

### D7. 同步與驗證流程
每完成一張圖或一節就執行一次：`olcli push` → `olcli pdf` → `olcli output log` → 在 log 中搜尋 `^!`、`undefined`、`Overfull`，並檢查 PDF 頁數與版面。Algorithm 和 TikZ 的錯誤通常會中斷編譯，所以每寫完一個就驗證一次，避免錯誤累積。本機進度以 git commit 保存；GitHub 只在完成後、經使用者同意才 push（見 `specs/paper-build`）。
- 替代方案：Overleaf Git 整合 → 專案回應 `no git access`，無法使用。本機安裝 BasicTeX → 使用者未採用。

### D8. 同步排除規則
Overleaf 端由 `.olignore` 控制，排除 `myRef/`、`openspec/`、`.claude/`、`README.md`、`overleafInstruction.md` 與 PDF/pptx/docx；GitHub 端由 `.gitignore` 控制，排除 `myRef/`、`.claude/` 與編譯產物。`.olignore` 本身納入 git 追蹤，讓排除規則有版本紀錄。

### D9. Article Summary Table 用跨雙欄的 `table*`
12 篇核心文獻 × 6 欄（Ref.、Year、AI Method、Domain / Dataset、Key Finding、Limitation），內容只取自 docx 3.3 節的彙整表；排除臨床指引 [12]、[31]，它們沒有 AI 方法與資料集，內容已在 II-D 節。6 欄放在單欄會過窄，所以用 `table*`。定義放在 `sections/02_related_work.tex` 開頭，使它出現在 Fig. 1 之後的下一個頁面頂端，與 Fig. 1 不在同一頁。表格放在 `tables/summary.tex`，取代 `tables/related.tex`。
- 替代方案：單欄表格只列 3–4 欄 → 會丟掉 Finding/Limitation，失去彙整表的意義，所以不採用。

## Risks / Trade-offs

- [6 頁內容太擠，或剛好卡在 6 頁多幾行] → 依 D1 的刪減順序處理；最後才微調 `\vspace` 或圖的尺寸。
- [`figure*` 浮動位置不理想，例如被推到最後一頁] → 把 `figure*` 的定義放在第 1 頁的內容中（Introduction 附近），讓它出現在第 2 頁頂端。
- [Fig. 1 與 Article Summary Table 兩個跨欄浮動體互相推擠，例如表格被推到第 4 頁以後，離 II 節太遠] → 調整 `table*` 的定義位置與 `[t]` 選項；必要時縮小表格字級為 `\scriptsize`。
- [TikZ 在 Overleaf 上編譯變慢或逾時] → 兩張圖的節點數控制在合理範圍；必要時加大 `olcli` 的 `--timeout`。
- [文獻核對失敗，造成引用缺口] → 依 `specs/paper-references`，先不引用，彙整後由使用者決定。
- [英文改寫讓意思偏離 docx] → 改寫時逐段對照 docx，數值只從 docx 取用。

## Migration Plan

不需要遷移。同步一律用 `olcli push`；若要退回，用 `git revert` 撤銷變更後再 `olcli push`。GitHub 上的 `c6931ff` / `71bd6ed` 保有模板版本。未使用的 `overleaf` remote 與 MCP 可在使用者同意後移除。

## Open Questions

（無。9 篇待核對文獻已全部核對成功。）
