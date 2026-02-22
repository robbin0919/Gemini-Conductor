# Gemini Conductor 標準使用流程指南 (SOP)

本文件說明如何使用 **Gemini Conductor** 進行「上下文驅動開發 (CDD)」與「規格驅動開發 (SDD)」。本流程旨在確保 AI 在開發時擁有完整的專案脈絡，並嚴格遵循既定架構，防止程式碼品質失控。

---

## 📑 快速導覽 (Table of Contents)

<details>
<summary>點擊展開目錄 (Click to expand)</summary>

*   [🗺️ 開發導航：學習藍圖 (Project Blueprint)](#-開發導航學習藍圖-project-blueprint)
*   [🛠️ 第一階段：專案初始化與環境設定 (Setup)](#-第一階段專案初始化與環境設定-setup)
*   [🛤️ 第二階段：開啟開發軌跡 (New Track)](#-第二階段開啟開發軌跡-new-track)
*   [💻 第三階段：執行實作與驗證 (Implement & Review)](#-第三階段執行實作與驗證-implement--review)
*   [🔄 第四階段：異動處理與任務結案](#-第四階段異動處理與任務結案)
*   [🛑 隔離與安全規範 (Monorepo 適用)](#-隔離與安全規範-monorepo-適用)
*   [❓ 常見問題 (Q&A)](#-常見問題-qa)
    *   [Q1: 如果已經在 product.md 手動建立連結，還需要執行指令嗎？](#q1)
    *   [Q2: .conductor/product.md 應該如何撰寫？(範例)](#q2)
    *   [Q3: .conductor/architecture.md 應該如何撰寫？(範例)](#q3)
    *   [Q4: .conductor/conventions.md 應該如何撰寫？(範例)](#q4)
    *   [Q5: .conductorignore 應該如何撰寫？(範例)](#q5)
    *   [Q6: 核心設定檔可以合併寫在同一個檔案嗎？](#q6)
    *   [Q7: 軌跡目錄名稱的內容是如何決定的？可以中文嗎？](#q7)
    *   [Q8: 產出的 spec.md 與 plan.md 是空檔嗎？](#q8)
    *   [Q9: spec.md 與 plan.md 的範例內容為何？(範例)](#q9)
    *   [Q10: 若覺得產出檔案內容需要加強或有誤，該如何處理？](#q10)
    *   [Q11: /conductor:implement 讀取 plan.md 的具體規則為何？](#q11)
    *   [Q12: /conductor:review 的安全掃描規則與依據為何？](#q12)
    *   [Q13: 在 Monorepo 架構下，如何確保專案間的隔離與安全？](#q13)

</details>

---

## 🗺️ 開發導航：學習藍圖 (Project Blueprint)

以下是專案的完整目錄結構示意圖，展示了 Conductor 管理檔案、專案原始碼以及任務軌跡之間的相對位置：

```plaintext
Your-Project-Root/
├── 📁 .conductor/              <-- [初始化產出] Conductor 核心管理目錄
│   ├── 📄 product.md           <-- 專案願景、目標使用者與核心功能定義
│   ├── 📄 architecture.md      <-- 技術選型（語言、框架、資料庫）與架構規範
│   ├── 📄 conventions.md       <-- 程式碼風格、命名規範與錯誤處理家法
│   └── 📁 tracks/              <-- [任務容器] 存放各個開發軌跡的子目錄
│       └── 📁 track-001-.../   <-- [任務執行時產出] 獨立任務資料夾
│           ├── 📄 spec.md      <-- 該任務的具體功能規格
│           └── 📄 plan.md      <-- 該任務的詳細實作步驟
├── 📁 src/                     <-- 您的專案原始碼 (C#, TS, Python 等)
├── 📁 docs/                    <-- [收納建議] 存放各類設計與領域知識文件
│   └── 📄 OpenSearch_Design.md  <-- [源頭真理] 您撰寫的設計文件 (Context)
├── 📄 .conductorignore         <-- [手動建立] 限制 AI 視野的白名單設定
└── 📄 .gitignore               <-- 建議將 .conductor/tracks/ 納入版本控制
```

---

## 🛠️ 第一階段：專案初始化與環境設定 (Setup)

在開始開發前，必須先建立專案的「地基」。

1.  **進入目標專案目錄**
    若在 Monorepo 架構中，請務必 `cd` 進入具體的子專案目錄以進行隔離。
    ```bash
    cd path/to/your-project
    ```

2.  **執行初始化指令**
    ```bash
    /conductor:setup
    ```
    執行後，Conductor 會在目前目錄產出管理目錄與核心設定檔。其與專案程式碼的相對位置如下：

    ```plaintext
    Your-Project-Root/
    ├── 📁 .conductor/           <-- [初始化產出] 核心管理目錄
    │   ├── 📄 product.md        <-- 專案願景與核心目標
    │   ├── 📄 architecture.md   <-- 技術棧與架構規範
    │   ├── 📄 conventions.md    <-- 編碼風格與家法
    │   └── 📁 tracks/           <-- 任務軌跡容器 (初始為空)
    ├── 📁 src/                  <-- 您的專案原始碼
    └── 📄 ...                   <-- 其他專案檔案
    ```

    **核心設定檔說明：**
    *   **`.conductor/product.md`**: 專案的「靈魂」，讓 AI 了解商業邏輯與大局觀。
    *   **`.conductor/architecture.md`**: 技術藍圖，規定技術棧，防止 AI 擅自引入錯誤套件。
    *   **`.conductor/conventions.md`**: 開發守則，定義編碼風格（如：必須使用非同步 await）。
    *   **`.conductor/tracks/`**: 任務履歷容器，紀錄開發歷史，賦予 AI 長期記憶。
    *關於設定檔的「合併 vs 拆分」建議，請參考 [Q&A Q6](#q6)。*
    *關於各檔案的撰寫建議與範例：請參考 [Q&A Q2 (product.md)](#q2), [Q3 (architecture.md)](#q3) 與 [Q4 (conventions.md)](#q4)。*

3.  **定義「源頭真理」 (Source of Truth)**
    將收納在 `docs/` 的設計文件 (如 `OpenSearch_Design.md`) 關聯至 Conductor。您可以選擇在 `product.md` 建立超連結（推薦）或執行 `/conductor:context add` 指令。
    *詳細操作差異與長期記憶建議，請參考[常見問題 (Q&A) Q1](#q1)。*

4.  **手動建立忽略設定 (選配/建議)**
    初始化不會自動產生忽略檔案，建議手動配置：
    *   **`.conductorignore`**: (手動建立) 用於排除不相關目錄（如 Monorepo 中的其他專案），節省 Token 並防止幻覺。範例請參考[常見問題 (Q&A) Q5](#q5)。
    *   **`.gitignore`**: (檢查/手動建立) 確保 `.conductor/` 目錄及其下的 `product.md`, `architecture.md`, `conventions.md` 被納入版本控制；建議同時保留 `tracks/` 以維持長期記憶。

---

## 🛤️ 第二階段：開啟開發軌跡 (New Track)

將每個功能開發或 Bug 修復視為一個獨立的「軌跡 (Track)」。

1.  **發起新任務**
    ```bash
    /conductor:track "實作使用者登入驗證模組"
    ```

2.  **生成規格與計畫 (Spec & Plan)**
    執行後，Conductor 會在 `.conductor/tracks/` 下建立獨立的任務資料夾。其與原始碼的相對位置如下：

    ```plaintext
    Your-Project-Root/
    ├── 📁 .conductor/
    │   └── 📁 tracks/
    │       └── 📁 track-xxx-name/    <-- [本次任務產出] 獨立任務資料夾
    │           ├── 📄 spec.md        <-- 該任務的具體規格
    │           └── 📄 plan.md        <-- 該任務的執行計畫
    ├── 📁 src/                       <-- 您的專案原始碼
    └── 📄 ...
    ```
    *註：`track-` 為固定前綴；`xxx` 為自動遞增流水號；`name` 為任務描述的 URL 化字串。*

    *   **`spec.md`**: **規格書**。詳述任務需求、輸入輸出規格、異常處理場景與驗證標準。它是 AI 實作的最高準則。
    *   **`plan.md`**: **執行計畫**。由 AI 根據規格書拆解出的具體實作步驟（SOP）。在執行實作前，必須由人類審核確認。
    *關於目錄命名邏輯、擬稿機制與檔案範例內容，請參考 [Q&A Q7, Q8 與 Q9](#q7)。*

3.  **人類審核 (Checkpoint)**
    **⚠️ 重要：** AI 會在產出計畫後暫停。請務必檢查 `plan.md` :
    *   是否符合 `architecture.md` 的規範？
    *   是否遺漏了必要的測試案例？
    *   步驟是否過於籠統？（若太籠統，請要求 AI 拆解得更細）。

---

## 💻 第三階段：執行實作與驗證 (Implement & Review)

1.  **開始編碼**
    確認計畫無誤後，輸入指令讓 AI 執行：
    ```bash
    /conductor:implement
    ```
    **執行過程說明：**
    *   **計畫掃描**: AI 讀取 `plan.md`，識別第一個尚未完成的 `[ ]` 步驟。具體掃描規則請參考 [Q&A Q11](#q11)。
    *   **脈絡載入**: 自動加載相關原始碼、家法規範與 `spec.md`。
    *   **單步開發**: 針對當前步驟產生程式碼，並支援 TDD 模式（先寫測試）。
    *   **進度回填**: 任務完成後，自動將 `plan.md` 中的 `[ ]` 更新為 `[x]`。
    *   **自我修正**: 若遇到編譯錯誤，AI 會嘗試根據 Error Log 自行修復。

2.  **自動化審查 (Automated Review)**
    實作完成後，要求 AI 進行自我校驗：
    ```bash
    /conductor:review
    ```
    **執行過程說明：**
    *   **三方對比**: AI 會將「原始碼」與「`spec.md` (需求)」及「家法規範 (風格)」進行深度對照。
    *   **安全掃描**: 檢查是否存在硬編碼密鑰、非同步鎖死或潛在的邏輯漏洞。詳細規則與依據請參考 [Q&A Q12](#q12)。
    *   **產出報告**: 標註通過項、改進建議（⚠️）與不合規項（❌）。
    *   **引導修正**: 根據報告建議，您可以要求 AI 針對特定問題進行自動重構。

---

## 🔄 第四階段：異動處理與任務結案

1.  **中途修正規格**
    若開發到一半發現設計有誤或計畫不周全：
    *   修改 `spec.md` 內容或計畫。
    *   要求 AI 更新計畫：`/conductor:updatePlan`。
    *   相關處理細節請參考 [Q&A Q10](#q10)。

2.  **完成並歸檔**
    任務完成後，程式碼變更會透過 Git 進行提交。軌跡文件應保留在 `conductor/tracks/` 下作為歷史記憶（ADR）。

---

## 🛑 隔離與安全規範 (Monorepo 適用)

若您的儲存庫有多個專案，請務必實施「物理與視野雙重隔離」，詳細實作策略與建議請參考 [Q&A Q13](#q13)。

```plaintext
# 封鎖所有兄弟專案目錄
../*

# 只放行目前開發的專案與共用庫
!../current-project/
!../shared-packages/
```

---

## ❓ 常見問題 (Q&A)

<a id="q1"></a>
### Q1: 如果已經在 `product.md` 手動建立連結，還需要執行 `/conductor:context add` 嗎？
**A1:** 不需要「必須」執行，但了解兩者差異有助於選擇最適合的開發模式：

*   **方式 A：手動建立超連結 (推薦 / 長期記憶)**
    直接在 `.conductor/product.md` 使用 `[描述](路徑)` 建立連結。
    *   **運作機制**: Conductor 的核心邏輯是 **「追隨連結 (Following Links)」**。啟動一個任務 (Track) 時，AI 的第一個動作是讀取 `product.md`。如果它在裡面看到 `[設計文件](docs/OpenSearch_Design.md)`，它會自動將這個路徑加入它的「待讀取清單」。
    *   **結論**: 這是 **長期記憶** 的最佳實踐。這能確保以後每個新任務，AI 都會主動去翻閱這些「憲法級」規範，一勞永逸。
*   **方式 B：執行掛載指令 (顯式 / 單次載入)**
    在終端機下達 `/conductor:context add [路徑]`。
    *   **運作機制**: 這是一種 **「強迫載入 (Explicit Loading)」**。強制 AI 「現在立刻」把某份文件吃進目前的對話脈絡（Working Memory）中。
    *   **特性**: 它是 **單次 / 顯式** 的。如果您沒在 `product.md` 寫連結，只下指令，那麼下一次開新軌跡時，AI 可能會「忘記」去讀它。
    *   **用途**: 適合臨時掛載不在 `product.md` 中的檔案（例如：某次的 Bug Log 或單次的測試數據）。

**💡 最佳實務建議：**
優先在 `product.md` 建立連結以維持 AI 的全域視野與長期記憶，日常開發中即可直接略過指令步驟，讓流程更精簡。

---

<a id="q2"></a>
### Q2: `.conductor/product.md` 應該如何撰寫？有範例可以參考嗎？
**A2:** Conductor 建議將此文件視為專案的「憲法」。根據專案複雜度選擇風格：

| 撰寫風格 | 適用場景 | 核心差異 |
| :--- | :--- | :--- |
| **精簡版 (Simplified)** | 小型專案、快速原型 | 僅包含願景、核心功能與文件連結。 |
| **詳細版 (Detailed)** | 中大型專案、Monorepo | 包含目標使用者、核心目標、嚴格的專案限制與參考路徑。 |

**【精簡版】範例內容：**
```markdown
# 專案：快速匯出工具
## 🌟 願景
一個簡單的 Python 腳本，用於將 JSON 轉為 CSV。
## 🔗 參考文件
* [格式規範](docs/format.md)
```

**【詳細版】範例內容：**
```markdown
# 專案名稱：OSDX (OpenSearch Data Xport)

## 🌟 產品願景 (Vision)
OSDX 旨在提供一個高效、穩定且開發者友善的 CLI 工具，用於自動化地從 OpenSearch 叢集中匯出海量數據（50 萬筆以上）。透過標準化的流程，解決傳統匯出工具在處理大量資料時容易產生的記憶體溢位與連線中斷問題。

## 🎯 核心目標 (Goals)
1. **穩定性**：必須能夠在低資源環境下穩定完成百萬等級的資料提取。
2. **準確性**：確保匯出的資料與 OpenSearch 原始索引內容 100% 一致。
3. **易用性**：提供直觀的 TUI (Terminal UI) 介面，讓開發者能快速配置查詢條件。

## 👥 目標使用者 (Target Audience)
*   **資安分析師**：需要匯出大量 Log 進行離線威脅獵捕。
*   **SRE 工程師**：需要定期備份索引資料或進行跨叢集遷移。
*   **資料工程師**：需要將 OpenSearch 資料清理後轉入資料湖。

## 🛠️ 關鍵功能 (Key Features)
*   **Scroll API 支援**：全面採用 Scroll API 機制以避開 OpenSearch 10,000 筆查詢限制。
*   **斷點續傳**：當網路異常中斷時，能紀錄最後的 Scroll ID 並嘗試恢復任務。
*   **多格式支援**：支援輸出為 CSV 與 JSON Lines 格式。
*   **進度追蹤**：即時顯示資料提取速度、已完成百分比與預估剩餘時間。

## 🛑 專案邊界與限制 (Constraints)
*   **非同步優先**：所有的網路 IO 與檔案寫入必須嚴格使用非同步 `await` 模式。
*   **無狀態設計**：工具本身不應持久化存取任何 OpenSearch 的認證資訊，統一透過環境變數傳遞。

## 🔗 核心參考文件 (Source of Truth)
在進行任何功能規劃 (Plan) 前，AI 必須優先參考以下文件：
1.  **[系統設計規格書](docs/OpenSearch_Design.md)**：定義了核心架構與 Scroll API 實作細節。
2.  **[領域知識指南](docs/Domain_Knowledge.md)**：關於 OpenSearch Query 語法與性能調優的背景。
```
*技巧：在「詳細版」的專案限制中加入硬性規定，能大幅提升 AI 產出品質。*

---

<a id="q3"></a>
### Q3: `.conductor/architecture.md` 應該如何撰寫？有範例可以參考嗎？
**A3:** 此文件的目的是界定**技術邊界**，確保 AI 產出的程式碼符合既定的架構模式。

**範例內容：**
```markdown
# OSDX 技術架構規範 (Architecture)

## 🏗️ 核心架構模式 (Pattern)
本專案採用 Clean Architecture (簡化版)：
* **Infrastructure 層**: 負責 OpenSearch 連線、檔案 IO 與環境變數。
* **Domain 層**: 定義海量資料處理核心邏輯與 Scroll API 調度。
* **UI 層**: 使用 Spectre.Console 實作 TUI 互動。

## 🛠️ 核心技術棧 (Tech Stack)
* **語言與框架**: .NET 8 (C# 12)。
* **終端機 UI**: 全面強制使用 Spectre.Console。**嚴禁使用原生 Console.WriteLine**。
* **JSON 處理**: 統一使用 System.Text.Json。
* **資料提取機制**: 必須實作 Scroll API，嚴禁使用 from/size 進行海量分頁。

## 📡 依賴注入 (Dependency Injection)
* 將 OpenSearch 服務、日誌紀錄抽離為獨立 Interface。
* 在 Program.cs 統一完成服務註冊。

## 📂 目錄結構規範
* `/src/Services`: 業務邏輯實作.
* `/src/Models`: POCO 類別。
* `/src/Interfaces`: 服務介面定義。
```
*技巧：明確列出「絕對禁止」事項（如嚴禁使用原生 Console），是規範 AI 風格最有效的方法。*

---

<a id="q4"></a>
### Q4: `.conductor/conventions.md` 應該如何撰寫？有範例可以參考嗎？
**A4:** 此文件是專案的「家法」，用於定義最細節的**編碼風格與最佳實踐**。

**範例內容：**
```markdown
# OSDX 編碼規範與慣例 (Conventions)

## 🔤 命名規範 (Naming)
* **類別與方法**: 一律 PascalCase (如: ExportDataAsync)。
* **變數與參數**: 一律 camelCase (如: scrollId)。
* **介面**: 必須以大寫 I 開頭 (如: IOpenSearchService)。

## 🚀 非同步編程 (Asynchronous)
* **強制 Await**: 嚴禁使用 .Result 或 .Wait()。
* **名稱結尾**: 非同步方法名稱必須加上 Async 尾綴。

## 🛠️ 錯誤處理 (Error Handling)
* **全域捕捉**: 核心邏輯必須包裹在 try-catch 中。
* **錯誤輸出**: 透過 Spectre.Console 以紅色文字顯示訊息。

## 🧹 清潔代碼 (Clean Code)
* **小函式原則**: 單一方法建議不超過 30 行。
* **早退原則 (Early Return)**: 優先處理錯誤情況，減少縮排。
```
*技巧：針對 AI 容易犯的錯（如忘記 Async 尾綴或巢狀 if）特別加強說明，效果顯著。*

---

<a id="q5"></a>
### Q5: `.conductorignore` 應該如何撰寫？有範例可以參考嗎？
**A5:** 這個檔案的邏輯與 `.gitignore` 相似，目的是將 AI 的視野限制在必要的範圍內，以節省 Token 並減少干擾。

**範例內容：**
```plaintext
# 1. 忽略所有目錄 (採用白名單模式)
/*

# 2. 開放目前專案目錄 (白名單)
!/src/
!/docs/
!/.conductor/

# 3. 忽略大型二進位檔案與相依性
*.exe
*.dll
/node_modules/
/bin/
/obj/

# 4. 忽略日誌與暫存檔
*.log
/temp/

# 5. Monorepo 專用：忽略其他專案
/Project_BBB/
/Project_CCC/
```
*技巧：採用「白名單模式」（先用 /* 封鎖全部，再放行必要目錄）是最安全且最省 Token 的做法。*

---

<a id="q6"></a>
### Q6: `product.md`, `architecture.md` 與 `conventions.md` 可以寫在同一個檔案嗎？
**A6:** 技術上是**可以的**，但了解何時該「合併」或「拆分」有助於維持 AI 的開發品質：

1.  **什麼時候可以合併？ (小型專案)**
    如果您開發的是小型腳本或快速原型，可以只保留一個 `product.md` (或 `context.md`)，並在裡面使用標題區分區塊。
    *   **優點**: 管理簡單，檔案數量少。
2.  **為什麼 Conductor 預設要拆分？ (中大型專案)**
    *   **注意力聚焦 (Focus)**: AI 在執行任務時，如果單一檔案過大（包含過多願景、商業邏輯），會稀釋 AI 對細節規範（如命名家法）的注意力權重。
    *   **Token 經濟與效能**: 拆分檔案能讓 Conductor 精確讀取必要部分。例如：生成實作計畫時，它僅需讀取 `architecture.md`，而不必反覆讀取不相關的使用者介紹。
    *   **模組化維護**: 當編碼慣例 (Conventions) 更新時，不會影響到穩定的系統架構說明。

**💡 實務建議：**
*   **初創期**: 可以寫在一起，建議檔案命名為 `product.md`。
*   **成長期**: 當單一文件超過 **300 行**，或規範變得細碎複雜時，請果斷將「技術架構」與「編碼規範」拆分出去。
*   **進階期 (獨立新增檔案)**: 若特定主題（如：`security.md`, `database.md`）內容極為專業且細緻，建議**獨立新增專屬檔案**。
    *   **優點**: 主題聚焦 (Topic Isolation)，防止 AI 因為長文本而稀釋了對核心規範的權重。
    *   **關鍵動作**: 每新增一個檔案，務必在 `product.md` 中手動建立連結，確保 AI 的導航路徑不中斷。

---

<a id="q7"></a>
### Q7: 軌跡目錄名稱 (`track-xxx-name`) 的內容是如何決定的？可以使用中文嗎？
**A7:** 目錄名稱由 Conductor 根據您的指令輸入動態產生，其決定邏輯如下：

1.  **`name` 內容是如何決定的？**
    *   **輸入來源**: 來自您執行指令時提供的描述字串，例如 `/conductor:track "我的任務名稱"`。
    *   **轉換規則 (Slugification)**: 小寫化、空格轉連字號 `-`、自動過濾特殊符號。
    *   **轉換範例**: 輸入 `"Add Login!"` ➔ 產出 `track-001-add-login`。
2.  **可以使用中文嗎？**
    *   **技術上支援**: 支持 Unicode，因此中文目錄是可行的。
    *   **風險提示**: 跨平台開發（如 Windows vs Linux）可能產生路徑亂碼，且部分自動化工具對中文路徑支援度較低。
3.  **實務建議**: 為了穩定性，建議優先使用 **英文小寫** 與 **連字號 `-`** 作為描述。

---

<a id="q8"></a>
### Q8: 產出的 `spec.md` 與 `plan.md` 是空檔嗎？或是 `/conductor:track` 會先擬出一版？
**A8:** **不是空檔**。Gemini Conductor 核心精神是「規劃優先」，其流程如下：

1.  **即時擬稿**: 執行指令後，AI 會根據描述與專案背景，**立即擬出第一版草案**。
    *   **`spec.md` (擬稿內容)**: 包含 AI 理解後的功能清單、輸入輸出規範、異常場景與驗證點。
    *   **`plan.md` (擬稿內容)**: 包含 AI 建議的具體實作步驟（SOP）。
2.  **為什麼要先擬稿？**
    *   AI 先說明它的理解，由開發者擔任「審稿人」。確保在動手實作（Implement）前，雙方對需求已達成共識，避免 AI 產生幻覺或偏離架構。
3.  **如何微調內容？**
    *   若草稿有誤，開發者可直接編輯 Markdown 檔案，或對 AI 指令：「更新計畫，加入對某情境的處理」。

---

<a id="q9"></a>
### Q9: `spec.md` 與 `plan.md` 的範例內容為何？
**A9:** 以下提供開發「CSV 匯出功能」時的規格書與計畫書範例：

**【spec.md 範例 — 定義「要做什麼」】**
```markdown
# 任務規格：實作 CSV 檔案寫入模組
## ✅ 功能需求
1. 串流寫入：使用 StreamWriter 進行逐列寫入，支援百萬級資料。
2. 欄位映射：需根據索引 Schema 自動決定 CSV 表頭。
## 📥 輸入規格
* IEnumerable<JsonElement> 資料批次；string filePath 輸出路徑。
## 🧪 驗證標準
* 匯出 10 萬筆資料，欄位無錯位且檔案總大小符合預期。
```

**【plan.md 範例 — 定義「怎麼做」】**
```markdown
# 執行計畫：CSV 匯出功能實作
## 🏁 第一階段：地基建立
- [ ] 在 /src/Interfaces 建立 ICsvExportService.cs 介面。
## 🛠️ 第二階段：核心邏輯開發
- [ ] 實作 WriteBatchAsync 方法，使用非同步串流寫入。
## 🧪 第三階段：測試與整合
- [ ] 撰寫單元測試並在 Program.cs 註冊 DI。
## 🧹 第四階段：品質審查
- [ ] 執行 /conductor:review 進行自動化程式碼審查。
```
*技巧：AI 實作完成一個步驟後，會自動將 `[ ]` 改為 `[x]`，方便追蹤進度。*

---

<a id="q10"></a>
### Q10: 若覺得 `spec.md` 與 `plan.md` 內容需要加強或有誤，該如何處理？
**A10:** 核心原則是「文件先行，代碼隨後」。請依以下情境處理：

1.  **需求異動 (修改 `spec.md`)**:
    *   **手動/指令**: 直接編輯檔案修改功能描述，或指令 AI 更新規格。
    *   **連動更新**: 規格改變後，必須執行 `/conductor:updatePlan` 指令，強制 AI 同步調整實作步驟。
2.  **執行計畫不周全 (修改 `plan.md`)**:
    *   **手動修正**: 您可以直接在 Markdown 中增加缺失的步驟（如：增加單元測試）。
    *   **對話微調**: 對 AI 說：「你的計畫漏掉了資料庫遷移，請將步驟拆解更細」。
3.  **實作中途發現錯誤**:
    *   **如何暫停**: 在終端機按下 `Ctrl + C` 即可中斷目前的實作任務。
    *   **接續還是重來？**: Conductor 具備「狀態記憶」。當您再次下達 `/conductor:implement` 時，AI 會掃描 `plan.md`，自動**跳過已標記為 `[x]` 的步驟**，從第一個 `[ ]` 開始接續。
    *   **如何強制重做**: 若修正後的規格影響到已完成的部分，請手動將對應步驟從 `[x]` 改回 `[ ]`，AI 就會針對該部分重新實作。

---

<a id="q11"></a>
### Q11: `/conductor:implement` 讀取 `plan.md` 的具體規則為何？
**A11:** AI 掃描計畫書時遵循以下「狀態驅動」邏輯，您可以藉此精確引導開發行為：

1.  **由上而下掃描**: AI 嚴格遵守檔案中的標題與列表順序，不會跨步執行。
2.  **鎖定第一個 `[ ]`**: AI 會自動忽略所有標記為 `[x]` 的已完成項，並將焦點鎖定在檔案中出現的**第一個**未勾選核取方塊。
3.  **單步執行原則**: 為了確保代碼品質，AI 每次執行通常僅處理一個 `[ ]` 任務。
4.  **自動回填進度**: 實作程式碼並寫入檔案後，AI 會自動將該步驟更新為 `[x]`。
5.  **手動干預權**: 
    *   **強制重做**: 將已完成的 `[x]` 改回 `[ ]`，AI 下次執行時會針對該步驟重新產出。
    *   **跳過步驟**: 手動將 `[ ]` 標記為 `[x]`，AI 將視為該任務已由人類手動完成。
    *   **修改邏輯**: 直接改動 `[ ]` 後方的文字描述，AI 會即時採用新的指令邏輯。

---

<a id="q12"></a>
### Q12: `/conductor:review` 的安全掃描規則與依據為何？
**A12:** AI 的安全審查是基於「雙層防護」邏輯，結合了專案規範與通用資安標準：

1.  **專案規範合規 (優先依據)**:
    AI 會嚴格檢查程式碼是否違反了您在 `.conductor/architecture.md` (如：認證管理規範) 或 `conventions.md` (如：日誌安全規範) 中定義的硬性規則。
2.  **秘密資訊偵測 (Secret Detection)**:
    自動掃描是否包含硬編碼 (Hardcoded) 的 API 金鑰、密碼、資料庫連結字串或 OAuth Token。
3.  **防範常見漏洞 (Common Vulnerabilities)**:
    *   **注入攻擊**: 檢查 SQL 查詢是否參數化，以及外部路徑輸入是否經過過濾。
    *   **資訊洩漏**: 審查 Log 輸出內容，確保不會洩漏 PII (個人身分資訊) 或內部系統敏感資訊。
4.  **資安知識庫 (OWASP 基準)**:
    AI 內建了對 OWASP Top 10 等通用資安風險的認知，能針對不安全的序列化或無效的授權邏輯發出預警。
5.  **自定義規則擴充 (如：Checkmarx)**:
    雖然 Conductor 本身不是專業的掃描工具，但它可以透過您提供的「規則與清單」，扮演安全標準的線上執行者。主要整合方式：
    *   **規則注入 (Rules Injection)**: 在 `.conductor/conventions.md` 中直接加入核心要求（範例：「參考 Checkmarx 標準，嚴禁使用不安全的 HTML 渲染方法，必須使用編碼後的資料」）。
    *   **建立專屬安全文件 (Security Blueprint)**: 建立如 `docs/security_checkmarx.md` 的檢核表並在 `product.md` 中連結。執行 `/conductor:review` 時，AI 會將其視為「評分表」逐項核對。
    *   **報告修復迴圈 (Feedback Loop)**: 將現有的 Checkmarx 報告掛載 (`/conductor:context add report.md`)，指令 AI 在下一個軌跡 (Track) 中優先修復標記為 High 的漏洞。

---

<a id="q13"></a>
### Q13: 在 Monorepo 架構下，如何確保專案間的隔離與安全？
**A13:** 面對包含多個專案的 Monorepo，建議採用「三位一體」的隔離防護：

1.  **物理隔離 (目錄切換)**:
    *   **規則**: 不要從 Monorepo 根目錄啟動。
    *   **作法**: 先 `cd` 進入具體子專案目錄（如 `AAA-project/`），再下達 `/conductor:setup`。
2.  **視野隔離 (白名單模式)**:
    *   **作法**: 在子專案目錄建立 `.conductorignore`，封鎖所有外部路徑，僅顯式放行必要目錄。關於 `.conductorignore` 應該如何撰寫與範例，可參考 [Q&A Q5](#q5)。
    *   **範例**: `../*` (封鎖全部) ➔ `!../shared-lib/` (放行共用庫)。
3.  **權限與規範宣告**:
    *   **作法**: 在 `.conductor/product.md` 明確告訴 AI 其權限範圍。
    *   **範例**: 「你擁有 `./src` 的 R/W 權限；對於 `../shared-lib` 僅限 Read-Only，絕對禁止修改」。

**💡 帶來的核心優勢：**
*   **防止幻覺**: 確保 AI 不會引用到其他不相干專案（BBB, CCC）的程式碼。
*   **成本控制**: 有效節省約 80% 的 Token 掃描量，提升回應準確度與速度。
