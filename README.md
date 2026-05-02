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

**安裝指令：**
```bash
npm install -g @google/gemini-cli
```
> [!NOTE]
> 請確保系統已安裝 Node.js (v18 以上版本)。

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

**安裝方式：**
```bash
gemini mcp add google-maps-platform-code-assist npx -y @googlemaps/code-assist-mcp@latest
```

---

# 🤖 Workshop 2: 打造 LINE Bot 並部署至 Cloud Run

本章節指導如何建立 LINE Bot、取得金鑰，並使用 `gemini-cli` 部署至 Google Cloud Run。

## 📁 範例程式碼
本實作使用以下範例專案：
*   **Repo:** [https://github.com/kkdai/bwai2026-sample](https://github.com/kkdai/bwai2026-sample)

```bash
git clone https://github.com/kkdai/bwai2026-sample
cd bwai2026-sample
```

## 🔑 第一階段：取得 LINE 開發金鑰
1. **建立官方帳號**：進入 [LINE Business ID](https://manager.line.biz/) 建立新帳號。
2. **啟用 Messaging API**：在設定中啟用，並建立 Provider。
3. **取得金鑰**：在 [LINE Developers Console](https://developers.line.biz/console/) 取得：
   - `Channel Secret` (Basic settings)
   - `Channel Access Token` (Messaging API -> Issue)

## ☁️ 第二階段：Google Cloud 專案設定與開通

在部署前，需要確保 Google Cloud 專案已開通相關服務。

1. **建立/設定專案：**
   ```bash
   gcloud config set project [YOUR_PROJECT_ID]
   ```
2. **開通必要 API：**
   執行以下指令以啟用 Cloud Run 與 Cloud Build：
   ```bash
   gcloud services enable run.googleapis.com cloudbuild.googleapis.com
   ```

## 🔐 第三階段：設定 OAuth 與身分驗證

Gemini CLI 需要正確的身分驗證才能進行部署。

1. **Gemini CLI 登入：**
   初次執行時選擇 **"Sign in with Google"**。
2. **OAuth 重新設定：**
   若需更換帳號或重新驗證，可使用：
   ```bash
   /auth
   ```
3. **ADC 驗證 (推薦)：**
   確保本機環境已通過應用程式預設認證：
   ```bash
   gcloud auth application-default login
   ```

## 🚀 第四階段：使用 Gemini CLI 部署至 Cloud Run

Gemini CLI 具備「自動化部署」的能力，你只需要用自然語言下達指令。

1. **啟動 Gemini CLI 互動模式：**
   ```bash
   gemini
   ```
2. **下達部署指令：**
   在對話框中輸入：
   > "請幫我將這個專案部署到 Cloud Run，區域選擇 asia-east1，並設定環境變數 LINE_CHANNEL_SECRET 與 LINE_CHANNEL_ACCESS_TOKEN。"
3. **確認執行：**
   Gemini CLI 會分析專案結構，生成 `gcloud run deploy` 指令，並在執行前詢問你的確認。

## ⚙️ 第五階段：完成 LINE Webhook 設定
1. 部署完成後，取得 Cloud Run 的 **Service URL**。
2. 回到 LINE Developers Console -> Messaging API。
3. 在 **Webhook URL** 填入 `[Service URL]/callback`。
4. 點擊 **Verify** 確保連線成功。
5. 開啟 **Use webhook** 選項。
6. **關閉自動回應**：在 LINE Official Account Manager 的「回應設定」中，將回應模式設為「聊天機器人」，並停用「自動回應訊息」。

> [!CAUTION]
> **Channel Secret** 與 **Channel Access Token** 屬於敏感資訊，請務必妥善保管，**切勿直接提交至 GitHub**。



