# Spec Delta

## Purpose

規範參考文獻庫的建立方式：把提案書 32 篇文獻轉成 IEEE 格式的 BibTeX，並核對出處有疑慮的條目，讓公開 repo 中的引用資訊可查證。

## ADDED Requirements

### Requirement: 完整的 BibTeX 文獻庫
`references.bib` SHALL 包含提案書第 6 章列出的 32 篇文獻，每篇使用正確的條目類型（`@article`、`@inproceedings`、`@book`、`@mastersthesis`、`@misc` 等）。模板原本的示範條目若不在 32 篇之內，MUST 移除。

#### Scenario: 條目數量
- **WHEN** 計算 `references.bib` 的條目
- **THEN** 共 32 筆，對應提案書的 [1]–[32]

#### Scenario: 只列出有引用的文獻
- **WHEN** 編譯論文
- **THEN** 文末參考文獻只包含內文有 `\cite` 的條目，且 BibTeX 沒有錯誤

### Requirement: 核對不確定的文獻
[2]、[9]、[13]、[16]、[17]、[18]、[19]、[30]、[32] 這 9 篇 SHALL 透過 arXiv ID、DOI 或官方網址上網核對作者、標題、年份與出處。核對失敗或與提案書不符的條目 MUST 列給使用者決定，在決定前不得被論文引用。

#### Scenario: 核對成功
- **WHEN** 某篇文獻的資訊與線上來源一致
- **THEN** 依線上來源寫入 BibTeX，並補上可取得的 DOI 或網址

#### Scenario: 核對失敗
- **WHEN** 某篇文獻在線上找不到，或資訊與提案書衝突
- **THEN** 回報給使用者，且在使用者決定前論文不引用該條目

### Requirement: 中文出處的處理
中文出處（[16]、[17]、[18]）SHALL 以英文翻譯標題，並在條目中加註 `(in Chinese)`，讓 pdfLaTeX 不需要 CJK 支援即可編譯。

#### Scenario: 中文文獻可用 pdfLaTeX 編譯
- **WHEN** 以 pdfLaTeX 編譯並引用 [16]–[18]
- **THEN** 參考文獻清單顯示英文標題與 "(in Chinese)"，沒有缺字或編碼錯誤

### Requirement: 引用的論點有出處
論文中的統計數字與外部研究結論 SHALL 引用提案書中對應的文獻，不得新增提案書以外的文獻。

#### Scenario: 統計數字有引用
- **WHEN** 論文提到外食比例、代謝症候群盛行率、高血壓人數、鈉低估的 CV 20–70% 等數字
- **THEN** 該句附上提案書中對應的引用
