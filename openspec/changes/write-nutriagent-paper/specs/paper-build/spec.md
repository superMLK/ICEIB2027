# Spec Delta

## Purpose

規範論文的編譯驗證、篇幅限制與發布流程：檔案透過 Overleaf Git 整合同步，編譯結果由使用者在 Overleaf 網頁上確認，GitHub 公開 repo 只在完成後以受控方式發布。

## ADDED Requirements

### Requirement: 乾淨編譯
論文 SHALL 能在 Overleaf 上以 pdfLaTeX 編譯成功，沒有 LaTeX error、undefined reference、undefined citation，且沒有 overfull hbox 警告。

#### Scenario: Overleaf 編譯結果乾淨
- **WHEN** 變更已 push 到 overleaf remote，使用者在 Overleaf 網頁按 Recompile
- **THEN** Overleaf 沒有顯示錯誤，Logs 面板中沒有 undefined reference、undefined citation 或 Overfull 警告

### Requirement: 6 頁篇幅
編譯出的 PDF SHALL 剛好 6 頁（含參考文獻），且第 6 頁的內容不少於半頁。

#### Scenario: 頁數
- **WHEN** 使用者在 Overleaf 檢視編譯出的 PDF
- **THEN** 總頁數為 6，最後一頁不是只有零星幾行

### Requirement: 同步範圍
Overleaf 與 GitHub SHALL 只接收 git 追蹤的檔案。`myRef/` 與 `.claude/` MUST 被 `.gitignore` 排除，不得被 commit，因此也不會出現在 Overleaf 或 GitHub。`README.md`、`openspec/`、`.gitignore` 允許出現在 Overleaf 的檔案樹中。

#### Scenario: git 追蹤清單
- **WHEN** 執行 `git ls-files`
- **THEN** 不包含 `myRef/` 或 `.claude/` 的檔案，也不包含 `.olignore`

### Requirement: 受控的同步與發布
每次同步到 Overleaf 前 SHALL 先建立 git commit，並先 `git pull overleaf master` 合併 Overleaf 上的變更，再執行 `git push overleaf main:master`。push 到 GitHub 公開 repo MUST 在論文完成後一次進行，且必須先取得使用者同意。

#### Scenario: 同步到 Overleaf
- **WHEN** 完成一個需要編譯驗證的任務
- **THEN** 先 commit、pull overleaf、再 push overleaf，然後請使用者 Recompile 並回報

#### Scenario: push 到 GitHub 前詢問
- **WHEN** 論文完成，且所有變更都已 commit
- **THEN** 先向使用者確認，取得同意後才執行 `git push origin main`
