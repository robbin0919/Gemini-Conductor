# Gemini Conductor 🚀

**Gemini Conductor** 是一個專為 AI 輔助開發設計的協作框架，旨在透過「上下文驅動開發 (CDD)」與「規格驅動開發 (SDD)」來提升開發效率、確保代碼品質，並減少 AI 產生幻覺（Hallucination）的風險。

---

## 🌟 核心理念

*   **CDD (Context-Driven Development)**: 確保 AI 在動手前擁有完整的專案脈絡、技術架構與商業邏輯。
*   **SDD (Spec-Driven Development)**: 嚴格遵循「規格先行，計畫隨後，代碼最後」的開發節奏。
*   **軌跡追蹤 (Trackable History)**: 每一項任務都有獨立的規格書與計畫書，形成可追溯的開發履歷。

---

## 🛠️ 快速開始 (Quick Start)

### 1. 初始化專案地基
進入您的專案目錄並執行：
```bash
/conductor:setup
```
這將建立 `.conductor/` 目錄，包含 `product.md`、`architecture.md` 與 `conventions.md`。

### 2. 開啟開發軌跡
當有新功能或 Bug 需處理時：
```bash
/conductor:newTrack "功能描述"
```
AI 會根據專案脈絡產出 `spec.md` (規格) 與 `plan.md` (計畫)。

### 3. 執行實作
審核計畫無誤後，啟動自動實作：
```bash
/conductor:implement
```

### 4. 自動化審查
實作完成後，要求 AI 進行自我校驗：
```bash
/conductor:review
```

---

## 📁 專案結構 (Project Structure)

```plaintext
Your-Project-Root/
├── 📁 .conductor/              <-- Conductor 核心管理目錄
│   ├── 📄 product.md           <-- 專案願景與核心功能定義
│   ├── 📄 architecture.md      <-- 技術選型與架構規範
│   ├── 📄 conventions.md       <-- 程式碼風格與命名規範
│   └── 📁 tracks/              <-- 存放各個開發軌跡 (Tasks)
│       └── 📁 track-001-.../   <-- 獨立任務資料夾
│           ├── 📄 spec.md      <-- 任務規格書
│           └── 📄 plan.md      <-- 執行計畫書
├── 📁 src/                     <-- 您的專案原始碼
└── 📄 Gemini_Conductor_Workflow_Standard.md  <-- 詳細 SOP 手冊
```

---

## 📖 深入了解

詳細的開發流程與常見問題，請參閱：
👉 [Gemini Conductor 標準使用流程指南 (SOP)](./Gemini_Conductor_Workflow_Standard.md)
