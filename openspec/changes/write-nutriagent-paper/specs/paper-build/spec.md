# Spec Delta

## Purpose

規範論文的編譯驗證、篇幅限制與發布流程：透過 olcli 把論文原始檔同步到 Overleaf 並取得編譯結果，GitHub 公開 repo 只在完成後以受控方式發布。

## ADDED Requirements

### Requirement: 乾淨編譯
論文 SHALL 能在 Overleaf 上以 pdfLaTeX 編譯成功，log 中沒有 LaTeX error、undefined reference、undefined citation，且沒有 overfull hbox 警告。

#### Scenario: 編譯 log 乾淨
- **WHEN** 執行 `olcli pdf` 並以 `olcli output log` 下載 log
- **THEN** log 中搜尋 `^!`、`undefined`、`Overfull` 都沒有結果

### Requirement: 6 頁篇幅
編譯出的 PDF SHALL 剛好 6 頁（含參考文獻），且第 6 頁的內容不少於半頁。

#### Scenario: 頁數
- **WHEN** 檢查 `olcli pdf` 下載的 PDF
- **THEN** 總頁數為 6，最後一頁不是只有零星幾行

### Requirement: 同步範圍
上傳到 Overleaf 的檔案 SHALL 只包含論文原始檔（`main.tex`、`references.bib`、`sections/`、`figures/`、`tables/`），由 `.olignore` 排除 `myRef/`、`openspec/`、`.claude/`、`README.md`、`overleafInstruction.md`。`myRef/` 與 `.claude/` MUST 被 `.gitignore` 排除，不得被 commit 到 GitHub；`.olignore` 本身納入 git 追蹤。

#### Scenario: Overleaf 上傳清單
- **WHEN** 執行 `olcli push --dry-run --show-ignored`
- **THEN** 上傳清單中沒有 `myRef/`、`openspec/`、`.claude/`、`README.md` 或 `overleafInstruction.md`

#### Scenario: git 追蹤清單
- **WHEN** 執行 `git ls-files`
- **THEN** 不包含 `myRef/` 或 `.claude/` 的檔案

### Requirement: 受控的同步與發布
寫作過程中 SHALL 以 `olcli push` 同步到 Overleaf，並可在每個步驟建立本機 git commit。push 到 GitHub 公開 repo MUST 在論文完成後一次進行，且必須先取得使用者同意。

#### Scenario: push 到 GitHub 前詢問
- **WHEN** 論文完成，且所有變更都已 commit
- **THEN** 先向使用者確認，取得同意後才執行 `git push origin main`
