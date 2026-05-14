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

## 透過 Google Cloud Shell 直接啟動

### 確認專案已經選擇好了

<img width="1139" height="193" alt="image" src="https://github.com/user-attachments/assets/1b601755-e770-4532-98e9-669e9c418816" />

### 確認 Project ID

<img width="796" height="193" alt="image" src="https://github.com/user-attachments/assets/c55b652e-c105-465b-a923-1e418f82830f" />

### 啟動 Google Cloud Shell

<img width="222" height="137" alt="image" src="https://github.com/user-attachments/assets/3a4d99e3-4e97-41a7-b3ab-b5e62aa1ebbc" />

### 確定專案是新建的

<img width="829" height="79" alt="image" src="https://github.com/user-attachments/assets/93b918d9-cea8-40ec-a7d7-7649131f393a" />

- 測試 gcloud

```
gcloud
```

- 測試 gemini

```
gemini
```

## 🔌 建議安裝的官方 MCP

#### 1. 原本就有 VertexMCP Server

<img width="593" height="272" alt="image" src="https://github.com/user-attachments/assets/95006c28-7fde-4e93-8a89-5dcd6c146c8e" />

### 2. Google Developer Knowledge API
為你的 AI 助手裝上官方知識庫。詳細教學：[部落格連結](https://www.evanlin.com/gemini-cli-developer-mcp/)

**設定步驟：**
1. 前往 [Google Developer Console](https://console.cloud.google.com/) 啟用 **"Developer Knowledge API"**。 ([網址]([url](https://console.cloud.google.com/marketplace/product/google/developerknowledge.googleapis.com))）
2. **取得 API Key：** 建立憑證 -> API 金鑰，並限制為 Developer Knowledge API。
<img width="797" height="244" alt="image" src="https://github.com/user-attachments/assets/09233f8b-a6a3-4c68-84f3-20fa56cbd562" />
<img width="583" height="790" alt="image" src="https://github.com/user-attachments/assets/c1aeb0cf-2f42-4905-bdf1-444612f99ca9" />


3. **安裝 MCP：**
```bash
gemini mcp add -t http -H "X-Goog-Api-Key: YOUR_API_KEY" google-developer-knowledge https://developerknowledge.googleapis.com/mcp --scope user
```

### 3. Google Maps Platform Code Assist
詳細教學：[部落格連結](https://www.evanlin.com/map-mcp-grounding/)

```bash
gemini mcp add -s user -t http maps-code-assist-mcp https://mapscodeassist.googleapis.com/mcp
```

---

## 🔍 驗證與測試 MCP 服務

安裝完成後，你可以透過以下測試指令來確認 MCP Server 是否已正確掛載並運作。

### 1. 測試 Google Developer Knowledge API
這個 MCP 讓 Gemini 能夠讀取最新的 Google 官方文件。

*   **測試 Prompt：**
    > "請幫我查詢 Google Cloud Run 的最新部署限制（Deployment Limits），並列出前三項。"
*   **驗證重點：**
    *   Gemini 應該會調用 `google-developer-knowledge` 工具。
    *   回覆內容應包含來自 `cloud.google.com` 或 `developer.android.com` 等官方網域的資訊。
    *   回覆末尾通常會附上參考來源連結。

### 2. 測試 Google Maps Platform Code Assist
這個 MCP 專門協助開發者撰寫 Google Maps 相關的程式碼與 API 整合。

*   **測試 Prompt：**
    > "我想在網頁中嵌入一個 Google 地圖，請幫我寫出一段基本的 JavaScript 程式碼來顯示地圖，中心點設在台北 101。"
*   **驗證重點：**
    *   Gemini 應該會調用 `google-maps-platform-code-assist` 工具。
    *   生成的程式碼應包含 Maps JavaScript API 的正確載入方式與初始化語法。
    *   它可能會主動提醒你關於 `API Key` 的設定以及相關的安全性建議。

### 💡 測試小技巧
在 `gemini` 互動模式下，你可以輸入 `/mcp list` 來查看目前所有已啟動的 MCP Server 列表，確保它們的狀態為 **Connected**。

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
