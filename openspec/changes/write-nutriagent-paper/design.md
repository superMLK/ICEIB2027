# Design

## Context

- 模板已是模組化結構：`main.tex` 只負責 `\input` 各節，內容在 `sections/00–05`、`tables/`、`figures/`。已用 pdfLaTeX 在 Overleaf 編譯成功（3 頁佔位內容）。
- 本機沒有 LaTeX，**唯一的編譯環境是 Overleaf**。檔案透過 Overleaf Git 整合同步（DIC4 repo 的 remote `overleaf`，Token 存在 macOS keychain，username `git`）。Git 整合不會編譯，**我無法讀取編譯結果**，必須由使用者在 Overleaf 網頁 Recompile 後回報。
- Overleaf 上目前有之前用 olcli 上傳的 9 個檔案，與本機 repo 的歷史無關，第一次同步需要合併。
- 已註冊 user-scope MCP `overleaf-AIoT_DIC4`（mcp-server-git），提供 status/diff/commit/log 等操作；它沒有 push 功能，push 仍用 Bash 的 git。
- 內容來源 docx 是中文，約 700 行文字，份量遠超 6 頁，需要大量取捨（取捨原則見 proposal 與 `specs/paper-content`）。
- `.gitignore` 尚未排除 `.claude/`；`.olignore` 已不再需要。

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
| II. Related Work | 1.6 | 4 個子題 + 既有方法比較表 |
| III. Proposed Scheme | 4.6 | 問題定義、Design Principles、Fig. 1（跨欄，約 0.9 欄 × 2）、能量預算、Algorithm 1、排序與接地回覆、Algorithm 2、偏好更新、Fig. 2 |
| IV. Experiments | 2.4 | 資料集、baseline、指標表、原型結果與實作狀態表、Discussion |
| V. Conclusion | 0.4 | — |
| References | 0.8 | 約 25 筆引用 |

超出時的刪減順序：先壓縮 Related Work 的文字 → 精簡流程圖節點 → 合併指標表與結果表 → 縮減 Discussion。不刪 Algorithm 1/2 與 Fig. 1。

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
每完成一張圖或一節就執行一次：`git commit` → `git pull overleaf master` → `git push overleaf main:master` → 請使用者在 Overleaf 按 Recompile，並回報三件事：(1) 是否有錯誤（紅色）、(2) Logs 面板中是否有 undefined reference/citation 或 Overfull 警告、(3) 目前頁數。Algorithm 和 TikZ 的錯誤通常會中斷編譯，所以仍然每寫完一個就驗證一次，避免錯誤累積。
- 替代方案：本機安裝 BasicTeX 自動驗證 → 使用者選擇不採用。
- 因為每次同步都需要 commit，寫作過程會產生多個 commit；GitHub 只在完成後推一次（見 `specs/paper-build`）。

### D8. 同步排除規則
Overleaf 和 GitHub 都只接收 git 追蹤的檔案，所以排除規則只靠 `.gitignore`：新增 `.claude/`（可能含本機設定），`myRef/` 已在其中。`.olignore` 刪除。`README.md`、`openspec/` 會同時出現在 Overleaf 與 GitHub。

## Risks / Trade-offs

- [6 頁內容太擠，或剛好卡在 6 頁多幾行] → 依 D1 的刪減順序處理；最後才微調 `\vspace` 或圖的尺寸。
- [`figure*` 浮動位置不理想，例如被推到最後一頁] → 把 `figure*` 的定義放在第 1 頁的內容中（Introduction 附近），讓它出現在第 2 頁頂端。
- [TikZ 在 Overleaf 上編譯變慢或逾時] → 兩張圖的節點數控制在合理範圍。
- [使用者在 Overleaf 網頁上直接修改，造成 push 被拒絕] → 每次 push 前先 `git pull overleaf master`；有衝突時先回報使用者再處理。
- [每次驗證都要等使用者回報，進度較慢] → 每次請求都明確列出要回報的三件事（錯誤、警告、頁數），減少來回。
- [Token 過期或被撤銷] → push 失敗時請使用者重新產生 Token，並在自己的終端機輸入，Token 不經過 Claude。
- [文獻核對失敗，造成引用缺口] → 依 `specs/paper-references`，先不引用，彙整後由使用者決定。
- [英文改寫讓意思偏離 docx] → 改寫時逐段對照 docx，數值只從 docx 取用。
## Migration Plan

1. 使用者在自己的終端機執行 `git fetch overleaf`，輸入 Token 存進 keychain。
2. `git pull overleaf master --allow-unrelated-histories` 合併 Overleaf 既有歷史（內容與本機相同，預期沒有實質衝突；若有，以本機版本為準）。
3. `git push overleaf main:master`，請使用者確認 Overleaf 仍可編譯模板。

退回方式：用 `git revert` 撤銷變更後再 `git push overleaf main:master`；GitHub 上的 `c6931ff` / `71bd6ed` 保有模板版本。

## Open Questions

- 9 篇待核對文獻若有失敗，要改用其他出處還是直接刪除引用——等核對結果出來後再請使用者決定，不影響其他任務。
