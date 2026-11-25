# ICEIB 2027 — IEEE Conference Paper

ICEIB 2027 投稿用的 IEEE 雙欄研討會論文模板（`IEEEtran` conference）。每個章節拆成獨立的 `.tex` 檔，方便多人分工，也方便交給 AI 逐節協助撰寫，不用動到 `main.tex`。

## 研討會資訊

> 以下資訊整理自 ICEIB 2027 官網，**投稿前請再到官網確認**。官網目前提到的是 Engineering Proceedings 的 Word 模板，IEEE track 是否有專用 LaTeX 模板尚未確認，這裡先用標準 `IEEEtran`。

| 項目 | 內容 |
| --- | --- |
| 研討會 | 7th ICEIB 2027 |
| 日期 | 2027 年 4 月 23–25 日 |
| 地點 | 淡江大學 |
| 語言 | 英文 |
| 全文頁數 | **4–8 頁** |
| 摘要投稿截止 | 2027 年 3 月 1 日 |
| 摘要錄取通知 | 2027 年 3 月 15 日 |
| 全文投稿截止 | 2027 年 4 月 20 日 |
| 出版 | IEEE 或 Engineering Proceedings |

## 檔案結構

```
.
├── main.tex                  # 主檔：只負責組合各章節，一般不需要改
├── references.bib            # 參考文獻（BibTeX）
├── README.md
├── sections/
│   ├── 00_abstract.tex       # 摘要與關鍵字
│   ├── 01_introduction.tex   # I. Introduction
│   ├── 02_related_work.tex   # II. Related Work
│   ├── 03_proposed_scheme.tex# III. Proposed Scheme
│   ├── 04_experiments.tex    # IV. Experiments and Discussion
│   └── 05_conclusion.tex     # V. Conclusion
├── figures/                  # 圖檔（建議用 PDF 向量圖）
└── tables/
    └── comparison.tex        # 實驗比較表
```

標題和作者寫在 `main.tex`，其餘內容都在 `sections/` 和 `tables/`。

## 編譯

- 編譯器：**pdfLaTeX**（Overleaf 預設值）
- 主文件：`main.tex`
- 圖片目前用 `example-image-a/b/c` 佔位。把自己的圖放進 `figures/`，再改 `\includegraphics{...}` 的檔名，例如 `architecture.pdf`。

## 6 頁論文的篇幅分配建議

| 章節 | 頁數 | 要回答的問題 |
| --- | --- | --- |
| Abstract | 0.3 | 什麼問題？用什麼方法？結果如何？有什麼意義？ |
| I. Introduction | 0.9 | 為什麼需要這個研究？ |
| II. Related Work | 0.75 | 前人做了什麼？還缺什麼？ |
| III. Proposed Scheme | 1.8 | 創新點是什麼？怎麼運作？ |
| IV. Experiments | 1.65 | 怎麼證明方法有效？ |
| V. Conclusion + References | 0.6 | 學到什麼？還有什麼沒解決？ |

## 各章節的寫作邏輯

- **Abstract**：整個研究的故事
- **I. Introduction**：背景 → 問題 → 研究缺口 → 貢獻
- **II. Related Work**：既有方法 → 比較 → 限制
- **III. Proposed Scheme**：架構 → 數學模型 → 演算法
- **IV. Experiments**：資料集 → 比較基準 → 評估指標 → 結果 → 討論
- **V. Conclusion**：發現 → 意義 → 限制 → 未來工作
- **References**：可查證的文獻依據

## 同步流程

本機資料夾同時連著 Overleaf（透過 [olcli](https://www.npmjs.com/package/olcli)）和這個 GitHub repo：

```bash
olcli pull                    # 抓 Overleaf 上的最新內容
olcli push                    # 把本機修改上傳到 Overleaf
olcli pdf                     # 在 Overleaf 編譯並下載 PDF

git add -A && git commit -m "..." && git push   # 推到 GitHub
```
