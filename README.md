# BwAI-2026
Slides and related material about BwAI2026 training.

## 🚀 New Gemini API Features

* **2025/08/05** - [[n8n][Gemini] 如何用 Gemini 2.5 來抓取 YouTube 字幕並且變成 n8n 自動流程](https://www.evanlin.com/til-n8n-gemini-youtube-transcript/)
* **2025/12/09** - [[Gemini 3.0][Google Search] 使用 Google Search Grounding API 搭配 Gemini 3.0 Pro 來打造新聞與資訊助手](https://www.evanlin.com/search-grounding/)
* **2025/11/06** - [[Python] 用 Python + Gemini File Search 打造智能文件助手 LINE Bot：讓 AI 幫你讀文件](https://www.evanlin.com/linebot-gemini-file-search/)
* **2026/03/26** - [[Gemini] Tool Combo 實戰：在單次 API 呼叫中結合 Maps Grounding 與 Places API 打造 LINE 聚會地點小幫手](https://www.evanlin.com/gemini3-flash-combo/)

---


# Workshop 1: 安裝 Gemini_CLI, Gcloud Console 與相關的 Google 官方 MCP 服務

## 安裝 Gcloud 與 Gemini CLI 工具

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

Gemini CLI 是一個強大的終端機工具，讓你直接在命令列與 Gemini 模型互動並管理 MCP Server。

**安裝指令：**

```Bash
npm install -g @google/gemini-cli
```

註：請確保系統已安裝 Node.js (v18 以上版本)。

**驗證安裝：**

```Bash
gemini --version
```

## 🛠 Gemini CLI 建議裝的官方 MCP

### 1. Google Developer Knowledge API 與 MCP Server
為你的 AI 助手裝上官方知識庫。詳細教學：[部落格連結](https://www.evanlin.com/gemini-cli-developer-mcp/)

**設定步驟：**
1. 前往 [Google Developer Console](https://console.cloud.google.com/) 啟用 **"Developer Knowledge API"**。
2. **取得 API Key：**
   - 導覽至「憑證 (Credentials)」頁面。
   - 點擊「建立憑證」 -> 「API 金鑰」。
   - 編輯金鑰，設定名稱（例如：`Dev-Knowledge-Key`）。
   - 在「API 限制」下選擇「限制金鑰」，並從清單中選擇 **Developer Knowledge API**。
3. **安裝 MCP 指令：**

```
gemini mcp add -t http -H "X-Goog-Api-Key: YOUR_API_KEY" google-developer-knowledge https://developerknowledge.googleapis.com/mcp --scope user
```

### 2. Google Maps Platform Code Assist (MCP)
詳細教學：[部落格連結](https://www.evanlin.com/map-mcp-grounding/)

**安裝方式：**

```
gemini mcp add google-maps-platform-code-assist npx -y @googlemaps/code-assist-mcp@latest
```

---

# 🤖 Workshop 2 - 打造 LINEBot 來幫你儲存檔案在 Google Drive 中

本章節指導如何取得開發所需的 **Channel Secret** 與 **Channel Access token**。

## 第一階段：建立官方帳號
1. 進入 [LINE Business ID](https://manager.line.biz/) 登入頁面。
2. 存取 **LINE Official Account Manager**，選擇「使用 LINE 帳號登入」或「使用商用帳號登入」。
3. **建立新帳號**：
   - 帳號名稱：你的 Bot 名稱。
   - 公司/團體名稱：可填寫個人。
   - 業種：選擇適合的分類。
   - 電子郵件信箱。
4. 確認資料無誤後點擊「送出」。

## 第二階段：啟用 Messaging API
1. 在後台右上角點擊「**設定**」，左側選單點選「**Messaging API**」。
2. 點擊「**啟用 Messaging API**」。
3. **選擇/建立服務提供者 (Provider)**：輸入開發商名稱。
4. 完成後，帳號即與 LINE Developers Console 連結。

## 第三階段：取得開發金鑰
請跳轉至 [LINE Developers Console](https://developers.line.biz/console/) 進行：

1. **取得 Channel Secret**：
   - 在 Console 中進入該 Channel，預設於 **Basic settings** 分頁。
   - 向下捲動到底部即可看到 `Channel secret`。
2. **取得 Channel Access Token**：
   - 點選 **Messaging API** 分頁。
   - 捲動到頁面最下方，找到 **Channel access token**。
   - 點擊「**Issue**」(發行)，取得產生的字串 (long-lived)。

## 第四階段：關閉自動回應（重要）
為了讓程式碼完全接管訊息，請進行以下設定：
1. 回到 LINE Official Account Manager。
2. 點擊右上角「**設定**」 > 「**回應設定**」。
   - **回應模式**：設定為「聊天機器人」。
   - **進階設定**：
     - **自動回應訊息**：停用。
     - **Webhook**：啟用（部署 Server 後開啟）。

> [!CAUTION]
> **Channel Secret** 與 **Channel Access Token** 屬於敏感資訊，請務必妥善保管，**切勿上傳到 GitHub 等公開環境**。


