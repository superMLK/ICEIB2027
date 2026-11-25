# Spec Delta

## Purpose

規範 NutriAgent 論文的文字內容：章節結構如何對應 A2Z SOP、內容來源、作者資訊，以及實驗數據與實作狀態的誠實揭露規則。

## ADDED Requirements

### Requirement: 單一內容來源
論文的技術內容 SHALL 只以 `myRef/NutriAgent_Proposal_第10組.docx` 為準。`NutriAgent 提案解析.pdf` 與 `營養智慧代理架構.pdf` 中與 docx 不一致的描述 MUST NOT 出現在論文中。

#### Scenario: 舊版技術描述不出現
- **WHEN** 檢查論文全文
- **THEN** 不出現 PyTorch Q network、Neural Contextual Bandit 的梯度訓練、Adam 優化器、MAE 114.55 kcal 或延遲 0.31 ms 等舊版內容

#### Scenario: 數值與 docx 一致
- **WHEN** 論文引用任何設計參數（例如 0.65 阻尼、900 kcal 上限、1.2/2.5 km 半徑、700 mg 鈉門檻、分數權重 0.50/0.35/0.15）
- **THEN** 該數值與 docx 中的對應數值相同

### Requirement: 語言與格式
論文 SHALL 以英文撰寫，使用 IEEE conference 格式（`IEEEtran` conference 選項）。

#### Scenario: 內文無中文字元
- **WHEN** 以 pdfLaTeX 編譯論文
- **THEN** 編譯成功，且 PDF 中沒有中文字元

### Requirement: 標題與作者
論文標題 SHALL 為 "NutriAgent: A Grounded Conversational LLM Agent for Real-Time Dietary Decisions Integrating Wearable IoT and Clinical Nutrition Rules"。作者 SHALL 依序為 Wei-Sheng Li、Huan Chen，單位為 Department of Computer Science and Engineering, National Chung Hsing University, Taichung, Taiwan。email MUST 使用 `@example.com` 佔位，不得出現真實 email。

#### Scenario: 作者區塊
- **WHEN** 檢視 PDF 第一頁
- **THEN** 只出現兩位作者 Wei-Sheng Li 與 Huan Chen，且單位為 NCHU CSE

#### Scenario: 不洩漏真實 email
- **WHEN** 在 repo 所有 `.tex` 檔中搜尋 `@`
- **THEN** email 格式的字串都以 `@example.com` 結尾

### Requirement: A2Z SOP 章節結構
論文 SHALL 包含 Abstract 與 I–V 五節，並依 A2Z SOP 安排各節敘事：Abstract（A–F）、I. Introduction（G–L）、II. Related Work（M–O）、III. Proposed Scheme（PA–PM 中適用的項目）、IV. Experiments（Q–V）、V. Conclusion（W–Z）。

#### Scenario: 摘要的六段故事
- **WHEN** 閱讀 Abstract
- **THEN** 依序可辨識出動機、挑戰、提出的方案、方法、實驗設計、結果或預期成果六個部分，且沒有引用、圖表或公式

#### Scenario: 緒論交代貢獻與章節安排
- **WHEN** 閱讀 Introduction
- **THEN** 包含條列的貢獻清單，並在最後一段以 `\ref` 交代 II–V 節的內容

### Requirement: Deep Research 內容拆散融入
提案書第 4 章的內容 SHALL 拆散融入其他章節，不得成為獨立章節：5 個思維模式 MUST 濃縮為第 III 節的 Design Principles；3 個專業爭論 MUST 出現在第 IV 節的 Discussion；10 個深度問題中只有「馬拉松後 1,800 kcal」與「車上震動假步數」兩題 SHALL 改寫為第 III 節的範例情境。

#### Scenario: 沒有獨立的 Deep Research 章節
- **WHEN** 檢查論文章節標題
- **THEN** 沒有名為 Deep Research 或 Design Rationale 的 section

#### Scenario: 設計原則與討論存在
- **WHEN** 閱讀第 III 與第 IV 節
- **THEN** 第 III 節有 Design Principles 小節涵蓋 5 個原則，第 IV 節的 Discussion 涵蓋 3 個爭論

### Requirement: 不編造實驗數據
論文中所有呈現為「已觀察到」的數字 SHALL 來自 docx 的原型檢核結果（5 組合成檔案、45 次離線評估：CVR/AVR 0.0%、熱量 MAE 114.48 kcal、平均延遲 0.08 ms、P95 延遲 0.18 ms）。docx 中的預期成果與目標值 MUST 明確寫成 expected outcomes、targets 或 hypotheses。

#### Scenario: 原型結果標示為功能檢核
- **WHEN** 閱讀實驗結果段落與表格
- **THEN** 文字明確說明這些數字是離線模式下的 functional verification，不是正式實驗結論，並指出 CVR/AVR 0% 是規則設計的必然結果

#### Scenario: 預期成果不寫成已發生
- **WHEN** 論文提到 Gemini Zero-Shot 會產生違規、接地率 100%、MAE ≤ 180 kcal 等內容
- **THEN** 使用 expect、target、hypothesize 等表達未驗證的措辭

### Requirement: 揭露實作狀態
論文 SHALL 描述完整的系統設計，並在第 IV 節提供 Implementation Status，逐項標明已實作或規劃中，至少涵蓋：Gemini 回覆的數值驗證、Gemini 意圖解析、語義評分時打亂候選順序、CKD 規則所需欄位、包含 LLM 的端到端延遲。

#### Scenario: 讀者能分辨已驗證範圍
- **WHEN** 讀者閱讀原型結果
- **THEN** 可以知道 45 次評估是在未啟用 Gemini 的離線模式下進行，沒有涵蓋 Algorithm 2 的 LLM 數值驗證路徑
