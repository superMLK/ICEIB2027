# Spec Delta

## Purpose

規範論文中圖、演算法、表格與公式的呈現方式，讓系統設計能在 6 頁雙欄版面內以向量圖與結構化元素清楚表達。

## ADDED Requirements

### Requirement: TikZ 系統架構圖
論文 SHALL 包含以 TikZ 繪製的英文系統架構圖，以跨雙欄的 `figure*` 放置。圖中 MUST 呈現 8 個核心模組（User Scenario、Query/Input、Planner & Reasoning Agent、Data Acquisition/Retrieval、Analytics Engine、Domain Knowledge Integration、Decision/Recommendation、Output & Interaction）、外部資料來源（穿戴裝置、健檢報告、GPS、Gemini API）與知識庫 K1–K4，並以不同線型區分資料流、Planner 調度、LLM 呼叫與回饋。

#### Scenario: 架構圖完整
- **WHEN** 檢視 Fig. 1
- **THEN** 可以看到 8 個模組、4 個外部來源、K1–K4 與圖例，且所有文字為英文

#### Scenario: 架構圖是向量圖
- **WHEN** 在 PDF 中放大 Fig. 1
- **THEN** 文字與線條不會出現像素化

### Requirement: TikZ 處理流程圖
論文 SHALL 包含以 TikZ 繪製的英文處理流程圖，放在單欄。流程圖 MUST 至少包含以下判斷點：回饋意圖、穿戴資料是否取得、候選數是否 ≥ 4、安全池是否為空、Gemini 是否可用、回覆數字是否與資料庫一致。

#### Scenario: 關鍵分支可辨識
- **WHEN** 檢視 Fig. 2
- **THEN** 可以從圖中追蹤到「安全池為空 → 回報 NO_COMPLIANT_MEAL、不放寬規則」與「數字不一致 → 改用模板回覆」兩條路徑

#### Scenario: 流程圖不超出單欄
- **WHEN** 編譯論文
- **THEN** Fig. 2 寬度不超過欄寬，且不產生 overfull box 警告

### Requirement: 兩段演算法
論文 SHALL 只包含兩段具行號的 pseudocode：Algorithm 1 為臨床與過敏原硬約束閘門，Algorithm 2 為混合排序與接地回覆。提案書的主流程與偏好更新 MUST NOT 以 pseudocode 呈現。

#### Scenario: 閘門演算法涵蓋所有規則
- **WHEN** 閱讀 Algorithm 1
- **THEN** 包含過敏原別名展開，以及高血壓、糖尿病前期、痛風、高 LDL、CKD 五類規則，門檻值與 docx 相同

#### Scenario: 接地回覆演算法涵蓋降級路徑
- **WHEN** 閱讀 Algorithm 2
- **THEN** 包含打亂順序的 LLM 語義評分、0.5/0.5 混合分數、綜合分數、模板回覆，以及數字不一致時退回模板的判斷

### Requirement: 以公式表達的模型
動態能量預算、綜合排序分數與偏好更新 SHALL 以編號公式呈現，並在內文中以 `\eqref` 引用。

#### Scenario: 公式涵蓋核心計算
- **WHEN** 檢視第 III 節
- **THEN** 至少有 TDEE 與當餐預算、綜合分數、回饋獎勵與權重更新的編號公式

### Requirement: 表格
論文 SHALL 包含以 `booktabs` 排版的表格，至少有：既有方法比較表、評估指標定義表、原型檢核結果表（含實作狀態）。表格標題 MUST 放在表格上方。

#### Scenario: 表格格式
- **WHEN** 檢視任何一張表
- **THEN** 使用三線表，標題在上方，且沒有超出版面的 overfull 警告
