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

### 1. 建立官方帳號
1.  進入 [LINE Business ID](https://manager.line.biz/) 登入頁面。
2.  存取 **LINE Official Account Manager**，選擇「使用 LINE 帳號登入」或「使用商用帳號登入」。
3.  **建立新帳號**：
    *   **帳號名稱**：你的 Bot 名稱。
    *   **公司/團體名稱**：可填寫個人。
    *   **業種**：選擇適合的分類。
    *   **電子郵件信箱**。
4.  確認資料無誤後點擊「送出」。

### 2. 啟用 Messaging API
1.  在後台右上角點擊「**設定**」，左側選單點選「**Messaging API**」。
2.  點擊「**啟用 Messaging API**」。
3.  **選擇/建立服務提供者 (Provider)**：輸入開發商名稱。
4.  完成後，帳號即與 LINE Developers Console 連結。

### 3. 取得開發金鑰
請跳轉至 [LINE Developers Console](https://developers.line.biz/console/) 進行：
1.  **取得 Channel Secret**：
    *   在 Console 中進入該 Channel，預設於 **Basic settings** 分頁。
    *   向下捲動到底部即可看到 `Channel secret`。
2.  **取得 Channel Access Token**：
    *   點選 **Messaging API** 分頁。
    *   捲動到頁面最下方，找到 **Channel access token**。
    *   點擊「**Issue**」(發行)，取得產生的字串 (long-lived)。

### 4. 關閉自動回應（重要）
為了讓程式碼完全接管訊息，請進行以下設定：
1.  回到 LINE Official Account Manager。
2.  點擊右上角「**設定**」 > 「**回應設定**」。
    *   **回應模式**：設定為「聊天機器人」。
    *   **進階設定**：
        *   **自動回應訊息**：停用。
        *   **Webhook**：啟用（部署 Server 後開啟）。

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

Gemini CLI 需要透過 Google OAuth 進行身分驗證，才能代表你操作 Google Cloud 資源。以下是詳細的設定步驟：

### 1. 設定 OAuth 同意畫面 (OAuth Consent Screen)
在建立憑證之前，必須先設定同意畫面：
1.  進入 [Google Cloud Console - OAuth 同意畫面](https://console.cloud.google.com/auth/consent)。
2.  **User Type**：選擇「**外部 (External)**」（若為 Workspace 帳號可選內部），點擊「建立」。
3.  **應用程式資訊**：
    *   **應用程式名稱**：例如 `My Gemini CLI`。
    *   **使用者支援電子郵件**：選擇你的 Gmail。
    *   **開發者聯絡資訊**：填入你的電子郵件。
4.  點擊「儲存並繼續」，其餘步驟（範圍、測試使用者）可直接跳過或點擊「儲存並繼續」。
5.  最後回到儀表板，點擊「**發布應用程式**」並確認，這樣 OAuth 才能正式運作。

### 2. 建立 OAuth 客戶端 ID (OAuth Client ID)
1.  進入 [Google Cloud Console - 憑證 (Credentials)](https://console.cloud.google.com/auth/clients)。
2.  點擊「**建立憑證**」 -> 「**OAuth 客戶端 ID**」。
3.  **應用程式類型**：選擇「**電腦版應用程式 (Desktop App)**」。
4.  **名稱**：自訂名稱（例如 `Gemini CLI Client`）。
5.  點擊「建立」，系統會彈出一個包含 `Client ID` 與 `Client Secret` 的視窗。
    *   *註：Gemini CLI 在初次登入時會自動處理認證流程，通常不需手動填入這些金鑰，但確保專案中有此憑證是必要的。*

### 3. 使用 Gemini CLI 進行登入
1.  **初次登入：**
    執行 `gemini` 指令時，選擇 **"Sign in with Google"**，這會開啟瀏覽器讓你選擇 Google 帳號。
2.  **ADC 驗證 (強烈建議)：**
    為了讓背景部署指令順利執行，請在本機終端機執行：
    ```bash
    gcloud auth application-default login
    ```
    這會將憑證儲存在本機，供 `gcloud` 與 `gemini-cli` 使用。
3.  **重新驗證：**
    若需切換帳號或權限過期，可輸入：
    ```bash
    /auth
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

---

# 🔍 驗證與測試 MCP 服務

安裝完成後，你可以透過以下測試指令來確認 MCP Server 是否已正確掛載並運作。

## 1. 測試 Google Developer Knowledge API
這個 MCP 讓 Gemini 能夠讀取最新的 Google 官方文件。

*   **測試 Prompt：**
    > "請幫我查詢 Google Cloud Run 的最新部署限制（Deployment Limits），並列出前三項。"
*   **驗證重點：**
    *   Gemini 應該會調用 `google-developer-knowledge` 工具。
    *   回覆內容應包含來自 `cloud.google.com` 或 `developer.android.com` 等官方網域的資訊。
    *   回覆末尾通常會附上參考來源連結。

## 2. 測試 Google Maps Platform Code Assist
這個 MCP 專門協助開發者撰寫 Google Maps 相關的程式碼與 API 整合。

*   **測試 Prompt：**
    > "我想在網頁中嵌入一個 Google 地圖，請幫我寫出一段基本的 JavaScript 程式碼來顯示地圖，中心點設在台北 101。"
*   **驗證重點：**
    *   Gemini 應該會調用 `google-maps-platform-code-assist` 工具。
    *   生成的程式碼應包含 Maps JavaScript API 的正確載入方式與初始化語法。
    *   它可能會主動提醒你關於 `API Key` 的設定以及相關的安全性建議。

## 💡 測試小技巧
在 `gemini` 互動模式下，你可以輸入 `/mcp list` 來查看目前所有已啟動的 MCP Server 列表，確保它們的狀態為 **Connected**。



