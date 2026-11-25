# Tasks

「編譯驗證」在本文件中指：`git commit` → `git pull overleaf master` → `git push overleaf main:master` → 請使用者在 Overleaf 按 Recompile 並回報 (1) 是否有錯誤、(2) Logs 中是否有 undefined reference/citation 或 Overfull 警告、(3) 目前頁數；三項都符合預期才算通過。

## 1. 準備工作

- [ ] 1.0 確認使用者已在自己的終端機輸入 Overleaf Token（`GIT_TERMINAL_PROMPT=0 git fetch overleaf` 成功），再以 `git pull overleaf master --allow-unrelated-histories` 合併 Overleaf 既有歷史並 `git push overleaf main:master`；請使用者確認 Overleaf 仍能編譯模板
- [x] 1.1 刪除 `.olignore`，在 `.gitignore` 加入 `.claude/`；驗證 `git status` 不再列出 `.claude/`，`git ls-files` 不含 `myRef/`、`.claude/`、`.olignore`
- [x] 1.2 在 `main.tex` 更新標題與作者（Wei-Sheng Li、Huan Chen，NCHU CSE，`@example.com` 佔位），並加入 `tikz`（`positioning`、`arrows.meta`、`shapes.geometric`、`fit`、`calc`）與兩張圖共用的顏色和節點樣式；以編譯驗證確認第一頁的標題與作者正確

## 2. 參考文獻

- [x] 2.1 把提案書 32 篇文獻轉成 BibTeX，覆蓋 `references.bib`，移除不在 32 篇內的示範條目；中文出處 [16][17][18] 翻譯標題並加註 `(in Chinese)`；驗證條目數為 32
- [x] 2.2 上網核對 [2][9][13][16][17][18][19][30][32]，依線上資訊修正並補上 DOI 或網址，失敗的條目標記 `% UNVERIFIED`；產出核對結果清單
- [x] 2.3 把核對失敗或有衝突的條目回報使用者並取得決定，依決定更新 bib；驗證剩下的 `% UNVERIFIED` 條目都不會被論文引用

## 3. 系統架構圖與流程圖（TikZ）

- [x] 3.1 在 `figures/architecture.tex` 以橫向佈局畫出英文架構圖（8 個模組、4 個外部來源、K1–K4、3 種線型與圖例），暫時以 `figure*` 放進 `sections/03_proposed_scheme.tex`；編譯驗證，並看 PDF 確認圖完整、字可讀、所有文字為英文
- [x] 3.2 在 `figures/flowchart.tex` 畫出單欄英文流程圖（含 spec 要求的 6 個判斷點、NO_COMPLIANT_MEAL 與模板退回路徑、回饋迴圈）；編譯驗證，確認寬度不超過欄寬

## 4. 撰寫各節內容

- [x] 4.1 撰寫 `sections/03_proposed_scheme.tex`：問題定義、Design Principles（5 個原則）、系統架構說明（引用 Fig. 1）、動態能量預算公式與 2 個範例情境、Algorithm 1 臨床閘門、混合排序與綜合分數公式、Algorithm 2 接地回覆、偏好更新公式、流程說明（引用 Fig. 2）；編譯驗證，並對照 docx 確認所有門檻值與權重一致
- [x] 4.2 撰寫 `sections/04_experiments.tex` 與 `tables/` 下的表格：資料集規劃、5 個 baseline、評估指標表、原型檢核結果與 Implementation Status 表、Discussion（3 個爭論）；刪除 `tables/comparison.tex`；編譯驗證，並確認所有預期成果都用 expect / target / hypothesize 措辭、原型數字與 docx 完全相同
- [x] 4.3 撰寫 `sections/02_related_work.tex` 與既有方法比較表：LLM Agent 與排序、健康感知推薦、LLM 營養估算、臨床指引與穿戴裝置四個子題；編譯驗證，確認引用的文獻都已核對或不在待核對清單中
- [x] 4.4 撰寫 `sections/01_introduction.tex`：G–L 結構、貢獻清單、以 `\ref` 交代章節安排；編譯驗證，確認統計數字都附有引用
- [x] 4.5 撰寫 `sections/05_conclusion.tex`（W–Z）與 `sections/00_abstract.tex`（A–F，150–250 字，無引用）；編譯驗證，並計算摘要字數

## 5. 篇幅調整與整體檢查

- [x] 5.1 依 design D1 的刪減順序把 PDF 調整到剛好 6 頁，並確認 Fig. 1 出現在第 2 頁頂端附近；編譯驗證並檢查頁數與最後一頁的內容量
- [x] 5.2 依 4 個 spec 的 scenario 逐項檢查：全文無中文字元、無舊版技術描述、email 只有佔位、沒有 Deep Research 章節、`git ls-files` 符合同步範圍（以上由我用 grep 與 git 指令檢查），以及 Overleaf 編譯乾淨（由使用者回報）；把檢查結果整理給使用者
- [ ] 5.3 確認所有變更都已 commit 並同步到 Overleaf（`git status` 乾淨、`git log overleaf/master` 與 `main` 一致）；接著詢問使用者是否 push 到 GitHub，取得同意後才 `git push origin main`
