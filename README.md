# BwAI-2026
Slides and related material about BwAI2026 training.

# Materials links


## New Gemini API Features

- [20250410 [Gemini][LINEBot] 透過 Google ADK 打造一個 Agent LINE Bot](https://www.evanlin.com/google-adk-linebot/)
- [20250805 [n8n][Gemini] 如何用 Gemini 2.5 來抓取 YouTube 字幕並且變成 n8n 自動流程](https://www.evanlin.com/til-n8n-gemini-youtube-transcript/)
- [20251209 [Gemini 3.0][Google Search] 使用 Google Search Grounding API 搭配 Gemini 3.0 Pro 來打造新聞與資訊助手](https://www.evanlin.com/search-grounding/)
- [20251106 [Python] 用 Python + Gemini File Search 打造智能文件助手 LINE Bot：讓 AI 幫你讀文件](https://www.evanlin.com/linebot-gemini-file-search/)
- [20260326 [Gemini] Tool Combo 實戰：在單次 API 呼叫中結合 Maps Grounding 與 Places API 打造 LINE 聚會地點小幫手](https://www.evanlin.com/gemini3-flash-combo/)

## Gemini CLI 建議裝的官方 MCP 
-
- [[Gemini CLI] Google Developer Knowledge API 與 MCP Server：為你的 AI 助手裝上官方知識庫](https://www.evanlin.com/gemini-cli-developer-mcp/)
  - 到 Google Developer Console 打開 "Developer Knowledge API"
  - 拿到相關 API Key
    - 在 Google Cloud 控制台導覽至「憑證 (Credentials)」頁面。
    - 點擊「建立憑證」，然後選擇「API 金鑰」。
    - 點擊「編輯 API 金鑰」。
    - 在名稱欄位輸入好辨識的名字（例如：Dev-Knowledge-Key）。
    - 在「API 限制」下，選擇「限制金鑰」。
    - 從 API 清單中選擇「Developer Knowledge API」，然後點擊確定。
    - 點擊「儲存」。 
  - 安裝 MCP 指令 `gemini mcp add -t http -H "X-Goog-Api-Key: YOUR_API_KEY" google-developer-knowledge https://developerknowledge.googleapis.com/mcp --scope user`
- [Google Maps Platform Code Assist (MCP)安裝教學與範例](https://www.evanlin.com/map-mcp-grounding/)
  - 安裝方式 `gemini mcp add google-maps-platform-code-assist npx -y @googlemaps/code-assist-mcp@latest`
- 

## 更多 AI 工程師需要工具

- [AI Engineers Starter Pack Vol.4](https://www.aiengineerpack.com/?success=1)
<img width="395" alt="image" src="https://github.com/user-attachments/assets/5bc31c86-f6c1-44cd-a5d0-67893242edca" />
