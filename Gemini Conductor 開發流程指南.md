# Q1: 說明如何使用 Gemini Conductor 程式開發

**Gemini Conductor** 是 Google 為其 **Gemini CLI** 開發工具推出的重要擴充功能（Extension）。它的核心設計理念是「**上下文驅動開發**」（Context-Driven Development），強調「先規劃、後實作」，將 AI 的開發流程從片段的聊天對話，轉向結構化、可版本控制的專案管理模式。

以下是使用 Gemini Conductor 進行程式開發的完整指南：

---

## 1. 核心觀念：從「聊天」轉向「規劃」

傳統 AI 開發常受限於對話紀錄的長度與暫時性。Conductor則引入了 **Tracks（軌跡）** 的概念，將開發目標固化為 Markdown 檔案：

*   **Context（上下文）**：定義產品目標、技術棧與程式碼規範。
*   **Specs（規格）**：詳細描述「要做什麼」以及「為什麼要做」。
*   **Plan（計畫）**：將任務拆解為具體的階段與子任務。

---

## 2. 安裝與初步設定

首先，你必須先安裝 `gemini-cli`，接著透過以下指令安裝 Conductor：

```bash
# 安裝 Conductor 擴充功能
gemini extensions install https://github.com/gemini-cli-extensions/conductor
```

### 專案初始化

進入你的專案資料夾後，執行：

```bash
/conductor:setup
```

Conductor 會掃描你的程式碼庫，自動識別開發語言與架構，並引導你建立 `product.md` 與技術規範文件。

---

## 3. 開發工作流：三步驟循環

### 步驟一：定義軌跡（Create a Track）

當你要新增功能或修復 Bug 時，先發起一個 Track：

```bash
/conductor:track "新增使用者登入功能"
```

AI 會與你進行對話，詢問細節並產出 `spec.md`（規格）與 `plan.md`（計畫）。這些檔案會儲存在 `conductor/tracks/<track_id>/` 下。

### 步驟二：執行實作（Implementation）

確認計畫無誤後，執行：

```bash
/conductor:implement
```

AI 代理人（Agent）會依照計畫中的順序開始撰寫程式碼。它支援 **測試驅動開發 (TDD)** 模式：

1.  撰寫測試程式。
2.  執行測試（確認失敗）。
3.  撰寫實作程式。
4.  再次執行測試（確認通過）。

### 步驟三：自動化審查（Automated Review）

這是 2026 年新增的強大功能。實作完成後，Conductor 會產出檢核報告：

*   **程式碼品質**：分析邏輯錯誤、非同步競態條件或空指標風險。
*   **規格合規性**：比對實作結果是否符合當初的 `spec.md`。
*   **安全性掃描**：檢查是否洩漏 API 金鑰或存在注入攻擊漏洞。
*   **測試驗證**：自動執行專案現有的單元測試並整合涵蓋率報告。

---

## 4. 為什麼選擇 Gemini Conductor？

| 特性 | 傳統 AI 聊天開發 | 使用 Gemini Conductor |
| :--- | :--- | :--- |
| **記憶與上下文** | 依賴對話紀錄，易遺忘 | 持久化 Markdown 檔案，隨時可中斷重啟 |
| **大型工作任務** | 難以處理複雜的功能 | 透過 Track 與 Plan 拆解階段任務 |
| **程式碼規範** | 每次都要重新提醒風格 | 自動讀取專案規範文件並強制執行 |
| **驗證機制** | 需手動複製貼上並測試 | 內建自動化審查與測試套件整合 |

# Q2: 這是在沒有用 SDD 或 CDD 情況下，將整理出專案是什麼 產出 `OpenSearch_Data_Export_Guide.md`，再依這份文件產出系統設計文件 `OpenSearch_Export_System_Design.md`，系統設計文件調整後定版後，再依這份系統設計進行開發，這樣重新改用 Gemini Conductor，要如何進行

若要將原本「手動撰寫指南 -> 手動撰寫設計文件 -> 根據文件開發」的傳統流程，轉換為 **Gemini Conductor** 的「上下文驅動開發 (CDD)」模式，您可以參考以下步驟進行遷移。

Conductor 的核心價值認在於將您現有的 `Guide.md` 與 `System_Design.md` 轉化為 AI 的**永久記憶（Context）**，使其在後續開發時能嚴格遵守既定規格。

---

### 第一階段：建立專案上下文 (Context Setup)

首先，您需要讓 Gemini Conductor 完整理解 OSDX 專案的背景與規範。

**初始化 Conductor 空間：**
在專案根目錄執行：
```bash
/conductor:setup
```

1.  **匯入現有文件作為「源頭真理」 (Source of Truth)：**
    將您已定版的 `OpenSearch_Data_Export_Guide.md` 與 `OpenSearch_Export_System_Design.md` 放入 Conductor 的知識庫路徑（通常是 `conductor/context/`）。
    *   **關鍵動作**：在 `product.md` 或 `architecture.md` 中明確引用這兩份文件。
    *   **提示詞範例**：*「本專案 OSDX 必須嚴格遵守 OpenSearch_Export_System_Design.md 定義的 .NET 8 架構與 Scroll API 機制。」*

---

### 第二階段：將開發任務拆解為「軌跡」 (Track & Plan)

傳統流程中，您可能直接開始寫 Code。在 Conductor 中，您要改為發起 **Tracks**：

**開啟功能軌跡：**
例如您要實作「Scroll API 資料分頁功能」，執行：
```bash
/conductor:track "實作基於 Scroll API 的海量資料匯出邏輯"
```

1.  **自動生成 Spec 與 Plan：**
    Conductor 會讀取您的 `System_Design.md`，自動產出該功能的 `spec.md` 與 `plan.md`。
    *   **優點**：它會自動知道要使用 .NET 8、`System.Text.Json` 以及處理 `max_result_window` 限制，因為這些都已經在您的設計文件中了。
2.  **審查計畫**：
    檢查 `plan.md` 中的步驟（例如：1. 實作 Scroll 初始化；2. 實作迴圈提取；3. 實作清除 Scroll ID），確認無誤後再繼續。

---

### 第三階段：實作與自動校驗 (Implementation & Review)

**自動實作 (Implement)：**
執行指令，讓 AI 根據計畫撰寫程式碼：
```bash
/conductor:implement
```

1.  AI 會自動在 C# 專案中產出符合 `Spectre.Console` 介面規範的程式碼。
2.  **自動事實檢查 (Fact-Check Review)：**
    這是最重要的改變。實作完成後，Conductor 會執行 **Review** 流程：
    *   **比對設計文件**：AI 會檢查寫出來的 `ExportService.cs` 是否真的用了 Scroll API（對照 `Guide.md` 第 3 點）。
    *   **檢查輸入格式**：檢查 Query 處理邏輯是否符合 `System_Design.md` 提到的「不包含外層 query 鍵」的校驗邏輯。

---

### 總結：改用 Conductor 後的流程變化

| 階段 | 傳統流程 (您目前的做法) | Gemini Conductor 流程 |
| :--- | :--- | :--- |
| **文件管理** | 檔案存在資料夾，開發時需人腦翻閱 | 文件轉化為 Context，AI 隨時主動參考 |
| **開發執行** | 對著設計文件手敲程式碼 | track -> plan -> implement (AI 自動化) |
| **品質控管** | 人工比對程式碼是否符合設計 | review 階段自動比對 `System_Design.md` |
| **知識延續** | 新成員加入需重讀兩份 Markdown | 新成員直接問 Conductor，AI 根據 Context 回答 |

# Q3: `/conductor:setup` 完成後，會產出那些檔案，如何進行 `OpenSearch_Export_System_Design.md` 內容貼給 Conductor

**在使用 Gemini Conductor 重新建構您的專案時，我們可以將其視為將「靜態文檔」轉化為「動態上下文」的過程。以下是針對您問題的詳細說明：**

### 1. `/conductor:setup` 執行後會產出的檔案

**當您在專案根目錄執行初始化指令後，Conductor 通常會建立一個 `.conductor/` 檔案夾（或類似名稱的隱藏目錄），其中包含以下核心架構：**

*   **`product.md` (或 `context.md`)：這是專案的「靈魂」。它定義了產品的願景、目標使用者以及核心功能概述。**
*   **`architecture.md`：記錄技術選型（例如您選擇的 .NET 8、`Spectre.Console`）。**
*   **`conventions.md`：定義程式碼風格與規範（例如命名規則、錯誤處理機制）。**
*   **`tracks/` 目錄：未來您每次執行 `/conductor:track` 所生成的規格書與計畫書都會存放於此。**

---

### 2. 如何將 `OpenSearch_Export_System_Design.md` 提供給 Conductor

**您不需要手動「貼上」整份文件的內容到對話框，Conductor 偏好直接讀取檔案。您可以透過以下兩種方式進行：**

#### 方式 A：直接指定檔案作為 Context（推薦）

**在初始化之後，您可以透過指令直接告訴 Conductor 參考這份文件：**

```bash
/conductor:context add OpenSearch_Export_System_Design.md
```

**或者，如果您正在發起一個新的開發任務（Track），可以直接在指令中提及：**

```bash
/conductor:track "根據 OpenSearch_Export_System_Design.md 實作核心匯出邏輯"
```

#### 方式 B：手動將內容整合進 Conductor 預設文件

**如果您希望長期維持 CDD（上下文驅動開發）的精神，建議將內容拆分填入：**

1.  **將 `OpenSearch_Export_System_Design.md` 的「設計目標」與「功能範圍」填入 `.conductor/product.md`。**
2.  **將「技術選型」與「架構設計」填入 `.conductor/architecture.md`。**
3.  **將「Query 校驗邏輯」與「Scroll API 規範」填入 `.conductor/conventions.md`。**

---

### 3. 改用 Conductor 後的具體操作流程範例

**既然您已經有了定版的系統設計文件，接下來的開發步驟如下：**

**第一步：啟動開發軌跡**

```bash
/conductor:track "實作 OSDX 的 Scroll API 資料提取模組"
```

**此時 Conductor 會問你：「我應該參考哪些規範？」 你回答：「請參考 `OpenSearch_Export_System_Design.md` 中的第 2 與第 3 章節。」**

**第二步：審查 AI 生成的 `plan.md` Conductor 會根據您的文件產出一個計畫，例如：**

1.  **建立 `OpenSearchClient` 類別。**
2.  **實作 `InitializeScroll` 請求（依據設計文件中的 JSON 格式）。**
3.  **實作 `FetchNextBatch` 迴圈。**
4.  **加入對 `max_result_window` 的例外處理。**

**第三步：執行實作與校驗**

```bash
/conductor:implement
```

**開發完成後，您可以執行：**

```bash
/conductor:review
```

**AI 會自動檢查：「現在寫出來的 C# 代碼，是否符合 `OpenSearch_Export_System_Design.md` 裡面要求的『不包含外層 query 鍵』的校驗邏輯？」 如果不符，它會主動修正。**

**這樣一來，您的 Markdown 文件就不再只是擺在旁邊看的「說明書」，而是變成了指導 AI 寫 Code 的「標準作業程序 (SOP)」。**

# Q4: 重新匯整，若是全新空白專案，是先 gemini CLI ,撰寫 `OpenSearch_Data_Export_Guide.md` 與 `OpenSearch_Export_System_Design.md` 這類的文件，再使用 `/conductor:setup` 初始化 Conductor 空間，再 `/conductor:context` 將文件作為 Context 嗎

這正是 **CDD (Context-Driven Development，上下文驅動開發)** 的標準工作流。在全新空白專案中，這種「文檔先行」的做法能確保 AI 產出的程式碼完全符合您的架構預期。

以下是為您整理的**標準作業順序**：

---

### 第一步：環境準備 (Gemini CLI)

在您的開發機器上安裝並登入 Gemini CLI。這是所有操作的基礎。

```bash
# 確保已安裝並登入
gemini auth login
```

### 第二步：撰寫核心定義文件 (The "Source of Truth")

在手動建立專案資料夾後，先撰寫您上傳的那兩份重要文件：

1.  **`OpenSearch_Data_Export_Guide.md`**：定義領域知識（Domain Knowledge），如 Scroll API 的運作原理。
2.  **`OpenSearch_Export_System_Design.md`**：定義技術規格（Spec），如 .NET 8、`Spectre.Console` 與核心邏輯。

**小撇步**：這時候還不需要寫任何程式碼（.cs 檔），只需確保這兩份 Markdown 內容完整。

### 第三步：初始化 Conductor 空間

在專案根目錄執行初始化指令。這會建立 Conductor 的管理結構（如 `.conductor/` 目錄）。

```bash
/conductor:setup
```

執行時，AI 可能會掃描目錄，發現您已經有兩份 Markdown，它會嘗試理解專案目標。

### 第四步：注入上下文 (Injecting Context)

這是最關鍵的一步，將您的設計文件正式掛載到 Conductor 的「大腦」中：

```bash
# 讓 Conductor 永久記住這份設計文件的規範
/conductor:context add OpenSearch_Export_System_Design.md
/conductor:context add OpenSearch_Data_Export_Guide.md
```

---

### 第五步：開始開發軌跡 (Track & Implement)

有了 Context 之後，您就不再需要下瑣碎的指令，而是以「任務」為單位：

**建立任務軌跡**：
```bash
/conductor:track "實作基於設計文件的 OpenSearch 連線與 Scroll 初始化模組"
```

1.  *此時，AI 會自動去讀 `System_Design.md`，知道要用 .NET 8 寫，且知道 Query 格式不能包含外層 `query` 鍵。*

**檢視並執行實作**： 確認 AI 產出的 `plan.md` 符合設計後，執行：
```bash
/conductor:implement
```

---

### 為什麼要這樣做？（優點總結）

*   **防止 AI 幻想 (Hallucination)**：AI 常常會用它自己學到的舊方法（例如用 `from+size` 分頁），但因為您先給了 `Guide.md`，它會被強制要求使用 **Scroll API**。
*   **一致性**：您的 `System_Design.md` 提到要用 `Spectre.Console` 做 TUI，Conductor 在實作 UI 互動時就會自動引用該套件，不會寫出傳統醜陋的 `Console.WriteLine`。
*   **自動審查標準**：後續您執行 `/conductor:review` 時，AI 會拿這兩份文件當作「評分表」來檢查程式碼。

# Q5: `Guide.md`、`System_Design.md`、`plan.md` 是 `/conductor:context` 會自已拆解歸檔嗎

**簡單來說：Conductor 不會自動幫你「搬移」或「拆解」檔案內容，但它會自動「理解並索引」它們。**

**為了讓您精準掌握自動化與手動的界線，以下詳細說明 Conductor 的運作邏輯：**

---

### 1. Conductor 如何處理您的 Markdown？

**當您執行 `/conductor:context add <檔案>` 時，Conductor 做的動作是「掛載（Mounting）」：**

*   **索引化**：它會掃描檔案內容，將其轉化為向量或結構化資料存入內部的上下文快照中。
*   **保持關聯**：它會記住這份檔案是「真理來源（Source of Truth）」。
*   **不會拆解**：它不會主動把 `System_Design.md` 裡面的文字剪下貼到 `.conductor/architecture.md`。

**為什麼不自動拆解？ 因為 Conductor 尊重您的原始文件版本控制。如果它幫你拆散了，當您更新 `System_Design.md` 時，內部對應的檔案就會過期。保持原始檔案存在，AI 才能隨時讀取最新版本。**

---

### 2. 檔案角色的定義與歸檔邏輯

**在 Conductor 的開發世界裡，這三類檔案有明確的生命週期：**

| 檔案類型 | 歸檔邏輯與位置 | 誰來維護？ |
| :--- | :--- | :--- |
| **Guide & System_Design** | **屬於 Long-term Context（長期上下文）。建議放在專案根目錄或 docs/。** | **您（開發者）。這是專案的憲法，定版後不常變動。** |
| **plan.md** | **屬於 Track Context（任務上下文）。存放在 .conductor/tracks/<id>/plan.md。** | **AI 生成，您審核。這只針對「當前任務」，任務完成後會隨 Track 歸檔。** |

---

### 3. 推薦的最佳實作流程 (Best Practice)

**既然 Conductor 不會自動拆解，您應該按照以下順序操作，以達到最強的開發自動化：**

#### Step 1: 初始化

```bash
/conductor:setup
```

**此時會產生空的 `product.md`。**

#### Step 2: 建立連結 (不要搬移檔案)

**您不需要把內容貼進 `product.md`，只需告訴 AI：**

```bash
/conductor:context add OpenSearch_Data_Export_Guide.md
/conductor:context add OpenSearch_Export_System_Design.md
```

#### Step 3: 在 `product.md` 寫入引用 (關鍵動作)

**手動編輯 `.conductor/product.md`，在裡面寫一行話：**

**「本專案開發規範與架構設計完全遵循 `OpenSearch_Export_System_Design.md` 與 `OpenSearch_Data_Export_Guide.md`。在生成任何計畫 (plan.md) 時，必須優先參考這兩份文件。」**

---

### 4. 當您執行 `/conductor:track` 時會發生什麼？

**這時 Conductor 的威力才會展現：**

1.  **它會去讀 `.conductor/product.md`。**
2.  **發現裡面叫它去看 `System_Design.md`。**
3.  **它會自動去翻閱 `System_Design.md` 裡的技術選型（.NET 8, Scroll API）。**
4.  **最後，它會自動產出一個本次任務專屬的 `plan.md`。**

**總結您的疑問：**

*   **`Guide.md` / `System_Design.md`：由您保留，Conductor 負責「讀取」。**
*   **`plan.md`：由 Conductor 在 `tracks` 資料夾下「產出」，任務結束後自動留存作為開發紀錄。**

# Q6: 有點概念，能繪製檔案目錄結構，以提昇理解

**！透過檔案目錄結構，您可以清楚看到 您的原始設計文件 與 Gemini Conductor 自動生成的管理檔案 是如何並存且互相協作的。**

**以下是一個採用 .NET 8 (C#) 開發 OSDX 專案 的標準 Gemini Conductor 目錄結構範例：**

### 📂 OSDX 專案目錄結構

```plaintext
OSDX/
├── 📄 OpenSearch_Data_Export_Guide.md     <-- [您撰寫的] 領域知識指南
├── 📄 OpenSearch_Export_System_Design.md  <-- [您撰寫的] 系統設計文件 (Context)
├── 📁 src/                                <-- 原始碼目錄
│   ├── 📄 OSDX.csproj
│   ├── 📄 Program.cs                      <-- 進入點 (使用 Spectre.Console)
│   └── 📁 Services/
│       └── 📄 ExportService.cs            <-- 實作 Scroll API 的核心邏輯
├── 📁 .conductor/                         <-- [/conductor:setup] 產出的管理目錄
│   ├── 📄 product.md                      <-- 專案總體目標 (需手動加入對 Design.md 的引用)
│   ├── 📄 architecture.md                 <-- 定義 .NET 8, Spectre.Console 等架構
│   ├── 📄 conventions.md                  <-- 程式碼規範 (例如：必須使用非同步 await)
│   └── 📁 tracks/                         <-- 存放開發任務紀錄
│       ├── 📁 track-001-setup-scroll/     <-- [/conductor:track] 產出的任務資料夾
│       │   ├── 📄 spec.md                 <-- 該任務的具體技術規格
│       │   └── 📄 plan.md                 <-- 該任務的實作步驟 (AI 生成)
│       └── 📁 track-002-csv-exporter/
│           ├── 📄 spec.md
│           └── 📄 plan.md
└── 📄 .gitignore
```

---

### 結構細節說明

#### 1. 您的「真理來源」(Source of Truth)

*   **`OpenSearch_Data_Export_Guide.md` 與 `OpenSearch_Export_System_Design.md` 放在根目錄。**
*   **作用：當您執行 `/conductor:context add` 時，AI 會讀取這些檔案，確保它寫出的代碼符合「不包含外層 query 鍵」等特殊要求。**

#### 2. `.conductor/` 目錄 (管理核心)

*   **這是在執行 `/conductor:setup` 後出現的隱藏資料夾。**
*   **`product.md`：您可以想像它是 AI 的「導讀手冊」。**
*   **`conventions.md`：AI 寫 Code 的「家法」。如果您在裡面寫「所有變數命名必須符合 C# PascalCase」，AI 就不會寫出 camelCase。**

#### 3. `tracks/` 資料夾 (開發履歷)

*   **這是 Conductor 最強大的地方。**
*   **`plan.md`：當您開啟一個任務（例如：實作 Docker 部署），AI 會先在這裡寫下：「1. 建立 Dockerfile 2. 使用 Alpine 基底 3. 設定進入點...」。**
*   **您可以先修改這個 `plan.md`，確認沒問題後，再叫 AI 執行 `implement`。**

---

### 如何讓結構跑起來？

**當您的目錄結構如上所示時，您的操作指令會變得非常簡潔：**

**注入靈魂：**
```bash
/conductor:context add OpenSearch_Export_System_Design.md
```

**派發工作：**
```bash
/conductor:track "根據設計文件第 5 點，實作 Query 校驗邏輯"
```

1.  **觀察產出**： 您會看到 `.conductor/tracks/` 下多了一個資料夾，裡面有 AI 幫您拆解好的 `plan.md`，這完全是根據您的 `System_Design.md` 內容生成的。

# Q7: `Guide.md` / `System_Design.md` 是指 `OpenSearch_Data_Export_Guide.md` / `OpenSearch_Export_System_Design.md` 嗎

**在前面的說明中，為了讓目錄結構看起更簡潔，我使用了簡寫。**

**在您的實際專案中，它們就是指您上傳的那兩份文件：**

1.  **`OpenSearch_Data_Export_Guide.md` (領域知識指南)**
2.  **`OpenSearch_Export_System_Design.md` (系統設計文件)**

---

### 如何在 Conductor 中精確掛鉤這兩份檔案？

**當您執行完 `/conductor:setup` 後，建議您按照以下具體步驟操作，這能讓 Gemini 完全「繼承」您在文件中定義的規則（特別是 Scroll API 的使用與 Query 格式校驗）：**

#### 1. 執行 Context 掛載指令

**在終端機輸入：**

```bash
/conductor:context add OpenSearch_Data_Export_Guide.md
/conductor:context add OpenSearch_Export_System_Design.md
```

#### 2. 編輯 `.conductor/product.md` (這是關鍵)

**打開初始化生成的 `.conductor/product.md`，建議將內容修改為如下形式，這樣 AI 每次思考時都會先去讀這兩份檔案：**

```markdown
# Project: OSDX (OpenSearch Data Xport)

## 核心目標

本專案是一個 .NET 8 開發的 CLI 工具，用於自動化匯出 OpenSearch 海量資料。

## 強制參考規範 (Context References)

在進行任何程式碼生成或計畫規劃時，必須嚴格遵守以下文件：

1. **技術架構與規格**：參考 `OpenSearch_Export_System_Design.md`。
   - 必須使用 .NET 8 與 Spectre.Console。
   - Query 輸入校驗必須符合文件中的「不包含外層 query 鍵」規範。

2. **領域知識**：參考 `OpenSearch_Data_Export_Guide.md`。
   - 匯出機制必須使用 **Scroll API** 以避開 10,000 筆限制。
```

---

### 這樣做的好處

**當您下次執行： `/conductor:track "實作 Query 輸入模組"`**

**Gemini Conductor 會自動執行以下邏輯：**

1.  **讀取 `product.md` -> 發現需要參考 `OpenSearch_Export_System_Design.md`。**
2.  **讀取 `OpenSearch_Export_System_Design.md` -> 發現裡面提到：「*錯誤範例：{ "query": ... } (請勿包含外層 query 鍵)*」。**
3.  **產出 `plan.md` -> 計畫中會自動包含一項：「*實作正則表達式或 JSON 解析，確保使用者輸入不含冗餘的 query 鍵*」。**

**這就是「上下文驅動開發」的威力：您不需要重複叮嚀 AI，它會自己去翻閱您準備好的設計藍圖。**

# Q8: `architecture.md` 要怎麼處理

**在 Gemini Conductor 的架構中，`.conductor/architecture.md` 扮演的是「技術藍圖與開發守則」的角色。**

**如果說 `product.md` 是告訴 AI「我們要解決什麼業務問題」，那麼 `architecture.md` 就是嚴格規定 AI「必須用什麼技術與寫法來實作」。**

**雖然您已經有了詳細的 `System_Design.md`，但將關鍵的技術選型抽離並條列在 `architecture.md` 中，能讓 AI 在每次生成程式碼（`/conductor:implement`）時，將這些規則視為最高指導原則，避免它自作主張引入不需要的套件或寫出不符合現代標準的語法。**

**以下是針對 OSDX 專案，您可以直接貼入 `.conductor/architecture.md` 的內容範例：**

```markdown
# OSDX 技術架構規範 (Architecture)

## 1. 核心技術棧

* **語言與框架**：.NET 8 (C#)。
* **目標環境**：跨平台支援 (Windows 11, Linux, Docker)。

## 2. 介面與套件規範

* **終端機 UI**：全面強制使用 `Spectre.Console` 處理所有終端機輸出、互動式選單 (Prompts) 與進度條顯示。**嚴禁使用原生的 `Console.WriteLine` 或 `Console.ReadLine`**。
* **JSON 處理**：全面採用內建的 `System.Text.Json`，不使用 `Newtonsoft.Json`。

## 3. 核心機制實作要求

* **資料提取機制**：與 OpenSearch 互動時，必須實作 Scroll API 機制以處理海量資料（50萬筆以上），絕對不可使用傳統的 `from` 與 `size` 分頁。
* **依賴注入 (DI)**：程式架構需採用標準的 .NET 依賴注入模式 (Dependency Injection)，將 OpenSearch 連線邏輯、檔案寫入邏輯等抽離為獨立的 Service 介面與實作。

## 4. 錯誤處理與日誌

* 必須具備完善的 `try-catch` 機制，並將錯誤訊息透過 `Spectre.Console` 格式化輸出。
```

---

### 這樣處理的好處是什麼？

1.  **套件精準度**：當您發起一個新的開發軌跡（例如 `/conductor:track "實作匯出進度條"`），AI 會因為這份文件的存在，自動知道要調用 `Spectre.Console.Progress`，而不會自己去寫一個陽春的迴圈印出 `...`。
2.  **架構一致性**：要求使用依賴注入（DI）可以確保 AI 產出的程式碼具有高可測試性與良好的結構，不會把所有邏輯都塞在 `Program.cs` 裡面。

**總結來說，您可以這樣配置您的 Conductor 環境：**

*   **`product.md`**：寫入專案目標，並聲明「請參考 `System_Design.md` 與 `Guide.md` 作為詳細規格」。
*   **`architecture.md`**：貼上上述的技術選型與框架限制。

**完成這兩個檔案的設定後，您的 Conductor 就具備了極高水準的上下文認知，隨時可以開始接手開發任務了。接下來，您打算從哪一個功能模組開始發起第一個 Track 呢？**

# Q9: 這我可以使用 gemini CLI 處理嗎

**可以的！您完全可以使用 Gemini CLI 來自動處理這些設定檔的撰寫，而不需要手動複製貼上。這正是命令列工具與 AI 結合的最大優勢。**

**您可以透過以下兩種主要方式，讓 CLI 幫您完成 `.conductor/architecture.md` 與 `product.md` 的建置：**

### 方法一：發起軌跡，讓 Conductor 自己更新自己 (最推薦)

**既然您已經有了 `System_Design.md`，您可以直接把它當作任務來源，讓 Conductor 的 Agent 去修改它自己的設定檔。**

**請在終端機輸入：**

```bash
/conductor:track "請讀取 OpenSearch_Export_System_Design.md，將其中的技術選型（如 .NET 8、Spectre.Console、Scroll API 等限制）整理並寫入 .conductor/architecture.md 中；同時將專案概述寫入 .conductor/product.md。"
```

**執行後：**

1.  **AI 會產出一份 `plan.md`（例如：計畫步驟 1 是讀取設計文件，步驟 2 是更新 architecture.md）。**
2.  **您審查計畫沒問題後，輸入 `/conductor:implement`。**
3.  **AI 就會自動將整理好的規範寫入那兩個檔案中。**

### 方法二：使用原生 Gemini CLI 搭配輸出轉向 (Piping)

**如果您希望更直接地產出內容並寫入檔案，可以使用基礎的 `gemini ask` 指令，將檔案作為上下文傳入，並把輸出結果導向（`>`）到目標檔案：**

```bash
# 生成架構規範並寫入 architecture.md
gemini ask "請將這份設計文件中的技術選型與開發規範，整理成精簡的 Markdown 條列格式，不要有多餘的寒暄語。" --files OpenSearch_Export_System_Design.md > .conductor/architecture.md
```

---

### 💡 實務上的小提醒

**即使您使用了 CLI 讓 AI 自動生成這些設定檔，它們本質上仍然是普通的純文字 Markdown 檔案。生成完畢後，強烈建議您用 VS Code 或任何文字編輯器打開 `.conductor/architecture.md` 快速看過一眼，確保 AI 沒有遺漏您最在意的關鍵（例如「絕對不可使用 from/size 分頁」這條鐵律）。**

# Q10: `OpenSearch_Data_Export_Guide.md` / `OpenSearch_Export_System_Design.md` 中途有調整，該如何處理

**在開發過程中，需求與設計文件的變更是非常正常的。Gemini Conductor 的「上下文驅動」特性，正好能完美應對這種情況。**

**當您的 `OpenSearch_Data_Export_Guide.md` 或 `OpenSearch_Export_System_Design.md` 發生變更時，請依照以下兩種情境來處理：**

### 情境一：相關功能的程式碼「尚未」開始撰寫

**如果您只是在規劃階段修改了文件，還沒有讓 AI 產出對應的 C# 程式碼，處理方式非常簡單：**

1.  **直接存檔**：在您的編輯器中修改並儲存這兩份 Markdown 檔案。
2.  **直接發起新任務**：由於 Conductor 是在每次執行 `/conductor:track` 時去讀取 `.conductor/product.md` 內引用的檔案，它會自動讀取到最新版本的文件。
    *   **指令範例**：`/conductor:track "實作匯出功能"`（此時 AI 就會採用您剛修改完的新規則）。

### 情境二：相關功能的程式碼「已經」寫好了

**如果 AI 已經根據「舊版」的設計文件寫好了程式碼（例如 `ExportService.cs`），而您後來修改了設計文件（例如將原本的輸出格式從 CSV 改成 JSON），AI 不會通靈自動去改程式碼。您需要透過 Track 來進行重構 (Refactoring)：**

1.  **直接存檔**：確保您的 Markdown 檔案已更新並儲存。
2.  **發起重構軌跡**：明確告訴 Conductor 文件已經更新，要求它比對並修改現有程式碼。

**指令範例：**
```bash
/conductor:track "OpenSearch_Export_System_Design.md 的第 5 點邏輯已更新。請讀取最新文件，並重構現有的 ExportService.cs 以符合新的規範。"
```

3.  **審核與實作**：
    *   Conductor 會產出一個 `plan.md`，內容可能包含「讀取最新設計文件」、「修改 `ExportService.cs` 的資料處理段落」等。
    *   確認計畫無誤後，執行 `/conductor:implement`，讓 AI 完成程式碼的修改。

---

### 常見疑問：需要重新執行 `/conductor:context add` 嗎？

**通常不需要。 當您初次執行 `/conductor:context add` 時，Conductor 記住的是「檔案的路徑與關聯」，而不是把當時的文字死板地拷貝進去。只要檔名與路徑沒有改變，AI 在後續的任務中都會即時去讀取該檔案當下的最新狀態。**

**這正是使用文件作為「源頭真理 (Source of Truth)」來開發的最大好處：文件即程式碼品質的保證。只要文件更新了，後續所有的開發與自動審查 (`/conductor:review`) 都會自動以新版文件為基準。**

# Q11: 如果是編輯 `.conductor/product.md`，還需要執行 `/conductor:context add` 嗎，執行 `/conductor:context add` 差別在那

### 🔄 Gemini Conductor 真實運作邏輯

**在真實的 Conductor 中，沒有 `add` 指令，一切都取決於您怎麼編輯 `.conductor/product.md` 以及裡面的「檔案超連結」。**

| 真實操作情況 | AI 的內心 OS (真實運作邏輯) | 結果 |
| :--- | :--- | :--- |
| **只把設計文件放在資料夾 (不編輯 product.md)** | **「專案目錄裡有 Guide.md 和 System_Design.md，但老闆沒說那是什麼。遇到任務時，我還是靠自己的預設知識寫 .NET 好了。」** | **❌ 失敗：AI 不會主動參考文件，可能會寫出傳統的 from/size 分頁，而非您要求的 Scroll API。** |
| **編輯 product.md，但沒有放檔案連結 (只用文字口頭交代)** | **「老闆叫我要遵守『設計文件』，但沒告訴我檔案在哪裡。我只好憑空猜測你們公司的設計規範是什麼...」** | **⚠️ 有風險：AI 知道有規範存在，但因為找不到實體檔案，很容易產生「AI 幻覺」自己發明架構。** |
| **編輯 product.md 並加上正確的 Markdown 連結 (最佳實踐)** | **「老闆在專案大綱裡特別給了超連結，指名這是最高守則。我每次接任務前，都會順著連結走過去，把這兩份文件仔細讀一遍再動工！」** | **✅ 完美：精準產出使用 Spectre.Console 與 Scroll API 的高品質程式碼。** |

---

### 總結來說

**您不需要對 CLI 下任何死板的載入指令。您只需要像在寫 Wiki 或 Notion 一樣，把 `.conductor/product.md` 當作專案的首頁，然後把重要的設計文件用超連結牽起來，AI 的大腦就自動連線了。**

# Q12: 這樣再重新說明 `OpenSearch_Data_Export_Guide.md` / `OpenSearch_Export_System_Design.md` 中途有調整，該如何處理

**既然我們已經釐清了 Conductor 是透過 `.conductor/product.md` 內的「超連結」來動態讀取實體檔案，那麼當您的 `OpenSearch_Data_Export_Guide.md` 或 `OpenSearch_Export_System_Design.md` 發生變更時，處理方式就變得非常直覺了。**

**請依照您目前的開發進度，分為以下兩種情境來處理：**

### 情境一：相關功能的程式碼「尚未」撰寫（或準備開發新模組）

**如果您只是在規劃階段修改了規格，或者正準備開發下一個全新的功能：**

1.  **直接修改並存檔**：在 VS Code 或您的編輯器中，直接修改這兩份 Markdown 文件並儲存。
2.  **正常發起新任務**：如同往常一樣執行 `/conductor:track "實作某某功能"`。

**運作邏輯： 當您下達 `track` 指令時，Conductor 會打開 `product.md`，順著裡面的超連結去硬碟裡讀取那兩份文件。因為您剛剛已經存檔了，所以它自然會讀取到最新版本的內容。您完全不需要下達任何重整或更新快取的指令。**

---

### 情境二：相關功能的程式碼「已經」寫好（需要重構現有程式）

**假設 AI 昨天已經根據舊版的設計文件寫好了 `.NET` 的 `ExportService.cs`。今天，您修改了 `System_Design.md`，把 Query 格式的校驗邏輯改了。**

**此時，雖然 AI 下次看文件會看到新版，但它不會自動去改已經寫好的 `.cs` 檔案。您必須主動發起一個「重構任務」：**

1.  **直接修改並存檔**：確保您的 Markdown 文件已更新並儲存。
2.  **發起重構軌跡 (Refactoring Track)**：明確告訴 Conductor 文件已經更新，要求它去檢查並修改現有的程式碼。

**指令範例：**
```bash
/conductor:track "OpenSearch_Export_System_Design.md 裡的 Query 輸入格式說明已更新。請讀取最新文件，並重構現有的 ExportService.cs，確保其驗證邏輯符合新規範。"
```

3.  **審核與實作**：
    *   Conductor 會循線讀取最新的設計文件，接著產出一個 `plan.md`（計畫內容會包含：檢查現有程式碼、修改 JSON 驗證段落等）。
    *   您確認 `plan.md` 沒問題後，執行 `/conductor:implement`，AI 就會動手把舊程式碼改成符合新規格的樣子。

---

### 總結

**在上下文驅動開發 (CDD) 的世界裡，「文件即是真理」。**

**文件的修改只要一存檔，對 Conductor 來說就是即時生效的。您唯一需要判斷的只有一件事：這項文件的修改，是否需要 AI 去翻修過去已經寫好的程式碼？**

*   **如果不需要（只是影響未來的新功能），您什麼都不用多做。**
*   **如果需要（必須改舊 Code），就開一個新的 `/conductor:track` 指派它去重構。**

# Q13: 如果文件修改很多章節，可能影響不指一支程式，該如何處理

**處理大規模的架構或需求變更，正是採用「上下文驅動開發 (CDD)」最能展現價值的地方，但也最容易踩到 AI 的能力極限（Context 視窗過載或注意力發散）。**

**如果您的文件有大幅度修改，且預期會牽動多支程式碼（例如：同時改了資料庫連線方式、又改了 UI 介面流程），千萬不要只開一個 Track 叫 AI「把整個專案全部照著新文件改掉」。這會導致 AI 產生的 `plan.md` 過於龐大，實作時容易遺漏細節，甚至把原本正常的程式碼改壞。**

**面對跨模組的大規模重構，最穩健的策略是：「先請 AI 做影響評估，再分而治之 (Divide and Conquer)」。**

**以下是具體的標準作業流程：**

### 步驟一：發起「影響範圍分析」軌跡 (Impact Analysis)

**在您存檔了大幅修改的 `OpenSearch_Export_System_Design.md` 後，先不要急著寫 Code。您可以開一個專門用來「健檢」的 Track，讓 AI 幫您抓出要改哪裡。**

**執行指令：**
```bash
/conductor:track "設計文件已大幅更新。請讀取最新文件，並盤點目前 src/ 目錄下的所有程式碼，列出哪些檔案受到新規格의影響？請產出一份重構任務清單。"
```

*   **預期結果**： AI 會產出一個 `plan.md`，裡面不會直接寫 Code，而是告訴您：「*發現 `ExportService.cs` 的 Query 邏輯需要改、`Program.cs` 的 UI 選單需要增加新選項...等*」。

### 步驟二：依據清單，拆解為「多個獨立的重構軌跡」

**拿到 AI 整理好的清單後，您就可以把大工程切成小塊，逐一發起 Track 進行重構。這樣能確保 AI 在每一次實作時，都能 100% 專注在單一模組的品質上。**

**重構任務 A (先處理底層)：**
```bash
/conductor:track "根據新版設計文件第 3 章，重構 ExportService.cs 裡面的 Scroll API 呼叫邏輯。"
```

*   ***(執行 implement 與 review 確認無誤)***

**重構任務 B (再處理介面)：**
```bash
/conductor:track "底層已更新。現在請根據設計文件第 5 章，修改 UI 模組，確保使用者輸入的 Query 格式符合新的驗證標準。"
```

*   ***(執行 implement 與 review 確認無誤)***

### 步驟三：善用 `/conductor:review` 進行回歸測試

**當所有小模組都重構完畢後，這是 Conductor 內建審查機制發揮作用的最佳時機。 因為您的 `product.md` 裡面有規定「必須嚴格遵守設計文件」，所以在每一個模組重構完執行 `/conductor:review` 時，AI  都會拿著您最新版的文件當作評分表，檢查剛剛寫的程式碼有沒有規。這相當於有了一位自動化的架構師幫您做 Code Review。**

---

### 💡 核心觀念總結

**在傳統開發中，牽一髮動全身的大改版最怕「改漏了」。 在 Conductor 中，您的文件就是防漏網。只要文件寫得清楚，AI 就能幫您精準掃描出所有需要修改的角落。把大任務切成多個明確、範圍限縮的小 Track，是確保 AI 產出高品質程式碼的不二法門。**
