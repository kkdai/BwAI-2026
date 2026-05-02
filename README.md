# BwAI-2026 🎓
Slides and related material about BwAI2026 training.

## 🚀 New Gemini API Features

*   **2026/03/26** - [[Gemini] Tool Combo 實戰：在單次 API 呼叫中結合 Maps Grounding 與 Places API 打造 LINE 聚會地點小幫手](https://www.evanlin.com/gemini3-flash-combo/)
*   **2025/12/09** - [[Gemini 3.0][Google Search] 使用 Google Search Grounding API 搭配 Gemini 3.0 Pro 來打造新聞與資訊助手](https://www.evanlin.com/search-grounding/)
*   **2025/11/06** - [[Python] 用 Python + Gemini File Search 打造智能文件助手 LINE Bot：讓 AI 幫你讀文件](https://www.evanlin.com/linebot-gemini-file-search/)
*   **2025/08/05** - [[n8n][Gemini] 如何用 Gemini 2.5 來抓取 YouTube 字幕並且變成 n8n 自動流程](https://www.evanlin.com/til-n8n-gemini-youtube-transcript/)

---

# 🛠 Workshop 1: 環境準備與官方 MCP 服務

本章節指導如何安裝 `gcloud`、`Gemini CLI` 以及相關的 MCP 服務。

## 📦 安裝 Gcloud 與 Gemini CLI

### 1. 安裝 Google Cloud SDK (gcloud)
`gcloud` 是與 Google Cloud 服務互動的核心工具，Gemini CLI 依賴它進行身分驗證。

*   **macOS (Homebrew):**
    ```bash
    brew install --cask google-cloud-sdk
    ```
*   **Linux (Debian/Ubuntu):**
    ```bash
    curl https://sdk.cloud.google.com | bash
    exec -l $SHELL
    ```
*   **Windows:**
    請下載並執行 [Google Cloud SDK 安裝程式](https://cloud.google.com/sdk/docs/install#windows)。

**初始化與登入：**
安裝完成後，請執行以下指令完成授權：
```bash
gcloud init
gcloud auth application-default login
```

### 2. 安裝 Gemini CLI
Gemini CLI 是直接在終端機與 Gemini 模型互動並管理 MCP Server 的強大工具。

> [!NOTE]
> 請確保系統已安裝 Node.js (v18 以上版本)。

```bash
npm install -g @google/gemini-cli
```

**驗證安裝：**
```bash
gemini --version
```

## 🔌 建議安裝的官方 MCP

### 1. Google Developer Knowledge API
為你的 AI 助手裝上官方知識庫。詳細教學：[部落格連結](https://www.evanlin.com/gemini-cli-developer-mcp/)

**設定步驟：**
1. 前往 [Google Developer Console](https://console.cloud.google.com/) 啟用 **"Developer Knowledge API"**。
2. **取得 API Key：** 建立憑證 -> API 金鑰，並限制為 Developer Knowledge API。
3. **安裝 MCP：**
```bash
gemini mcp add -t http -H "X-Goog-Api-Key: YOUR_API_KEY" google-developer-knowledge https://developerknowledge.googleapis.com/mcp --scope user
```

### 2. Google Maps Platform Code Assist
詳細教學：[部落格連結](https://www.evanlin.com/map-mcp-grounding/)

```bash
gemini mcp add google-maps-platform-code-assist npx -y @googlemaps/code-assist-mcp@latest
```

**驗證 MCP 狀態：**
在 `gemini` 互動模式下輸入 `/mcp list`，確認服務狀態為 **Connected**。

---

# 🤖 Workshop 2: 打造 LINE Bot 並部署至 Cloud Run

本章節使用以下範例專案，完整的 step-by-step 部署指南請直接參考 sample repo 的 README：

**👉 [https://github.com/kkdai/bwai2026-sample](https://github.com/kkdai/bwai2026-sample)**

```bash
git clone https://github.com/kkdai/bwai2026-sample
cd bwai2026-sample
```

## 📋 Workshop 2 流程總覽

```
[階段一] 取得 LINE 開發金鑰（Channel Secret + Channel Access Token）
      ↓
[階段二] Google Cloud 專案設定（啟用 Cloud Run、Firestore 等 API）
      ↓
[階段三] 設定 OAuth 與身分驗證（OAuth Consent Screen + Gemini CLI 登入）
      ↓
[階段四] 使用 Gemini CLI 部署至 Cloud Run（自然語言下達部署指令）
      ↓
[階段五] 完成 LINE Webhook 設定（填入 Service URL/callback）
```

> [!TIP]
> 詳細操作步驟、指令範例與常見問題，請參閱 [sample repo README](https://github.com/kkdai/bwai2026-sample#readme)。
