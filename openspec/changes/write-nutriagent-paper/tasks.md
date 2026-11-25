# Tasks

「編譯驗證」在本文件中指：`olcli push` → `olcli pdf` → `olcli output log`，確認 log 中搜尋 `^!`、`undefined`、`Overfull` 都沒有結果，並檢查 PDF 的頁數與版面。

## 1. 準備工作

- [x] 1.0 嘗試 Overleaf Git 整合（Overleaf 回應 `no git access`）後改用 olcli：重建 `.olignore` 排除 `myRef/`、`openspec/`、`.claude/`、`README.md`、`overleafInstruction.md`；驗證 `olcli push --dry-run --show-ignored` 的上傳清單只有論文原始檔
- [x] 1.1 在 `.gitignore` 加入 `.claude/`；驗證 `git status` 不再列出 `.claude/`，`git ls-files` 不含 `myRef/`、`.claude/`
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
- [x] 5.2 依 4 個 spec 的 scenario 逐項檢查：全文無中文字元、無舊版技術描述、email 只有佔位、沒有 Deep Research 章節、`git ls-files` 符合同步範圍、Overleaf 上傳清單（以上由我用 grep、git 與 `olcli push --dry-run` 檢查），以及編譯 log 乾淨；把檢查結果整理給使用者

## 6. Article Summary Table

- [x] 6.1 新增 `tables/summary.tex`：跨雙欄 `table*`，12 篇核心文獻（docx 3.3 節彙整表扣除 [12]、[31]）× 6 欄（Ref.、Year、AI Method、Domain / Dataset、Key Finding、Limitation），每列以 `\cite` 標示；刪除 `tables/related.tex`，並在 `sections/02_related_work.tex` 開頭引入新表；驗證表中每個數值都與 docx 3.3 節相同
- [x] 6.2 刪減 `sections/02_related_work.tex` 中與表格重複的逐篇介紹，依 design D1 的刪減順序調整篇幅；編譯驗證，確認剛好 6 頁、log 乾淨、Article Summary Table 位於 Fig. 1 之後的頁面頂端且與 Fig. 1 不同頁
- [x] 6.3 重跑 5.2 的全部檢查（無中文字元、無舊版內容、email 佔位、同步範圍、log 乾淨），並確認 32 篇文獻中被引用的條目都正確列出
- [ ] 6.4 確認所有變更都已 commit 且 Overleaf 已同步到最新（`git status` 乾淨、最後一次 `olcli push` 成功）；接著詢問使用者是否 push 到 GitHub，取得同意後才 `git push origin main`
