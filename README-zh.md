Language / 語言: [English](README.md) | [中文](README-zh.md)


```markdown
# n8n 免費雲端部署指南 (Hugging Face + Supabase + Upstash)

本專案旨在建立一個**完全免費**且強大的 n8n 自動化工作流環境。
透過結合 Hugging Face 的運算資源、Supabase 的持久化資料庫以及 Upstash 的 Redis 快取，實現高性能且不間斷的雲端自動化服務。

## 🌟 方案優勢

*   **完全免費**：不需要信用卡即可擁有專業級配置。
*   **強大硬體**：提供 **2 CPU、16GB RAM** 以及 50GB 的磁碟空間。
*   **低限制**：網路流量幾乎無限制。
*   **較長的活躍期**：相較於其他平台（如 Render 的 15 分鐘），Hugging Face 在無活動後 **兩天** 才會進入休眠。
*   **持久化儲存**：透過 Supabase 解決 Hugging Face 磁碟非持久化（Not Persistent）的問題，確保容器重啟後工作流與資料不丟失。
*   **AI Agent 強化**：整合 Upstash Redis，提供比內建 Simple Memory 更強大的長效記憶與 Session 處理能力。

---

## 🛠 準備工作

在開始之前，請先註冊以下服務的帳號：
1.  **[Hugging Face](https://huggingface.co/)**：用於託管 n8n 主程式。
2.  **[Supabase](https://supabase.com/)**：用於儲存 PostgreSQL 資料庫。
3.  **[Upstash](https://upstash.com/)**：用於託管 Redis 服務。

---

## 🚀 部署步驟

### 第一步：設置 Supabase 資料庫
1.  在 Supabase 創建一個新的 **Organization** 與 **Project**。
2.  **務必記住資料庫密碼 (Database Password)**，後續在 Hugging Face 設置時需用到。
3.  伺服器區域建議選擇 **美西 (US West - California)**。
4.  進入專案的 `Connect` 設定，選擇 `SQL` 欄位下的 **Transaction Pooler**。
5.  記錄以下資訊：`Host`、`User`、`Database name` 以及 `Port` (預設為 6543)。

### 第二步：在 Hugging Face 部署 n8n
1.  進入 Hugging Face 的 **Spaces** 頁面，搜尋 `n8n` 並找到由 `bing` 維護的模板。
2.  點選右上角三個點，選擇 **Duplicate this Space**。
3.  在設定頁面填入以下環境變數 (Variables)：
    *   `DATABASE_PASSWORD`: 你的 Supabase 密碼。
    *   `DATABASE_USER`: Supabase 提供的使用者名稱。
    *   `DATABASE_HOST`: Supabase 的 Host 位址。
    *   `N8N_ENCRYPTION_KEY`: 自定義一組金鑰用於加密憑證。
    *   `N8N_PORT`: 必須設定為 **7860** (Hugging Face 專用的通訊埠)。
4.  將 Space 設定為 **Private** 或 **Public**，然後點擊 **Duplicate Space** 開始構建。

### 第三步：登入與驗證
1.  等待狀態顯示為 `Running` 後，進入 Space 頁面。
2.  **重要提示**：請在 **Logs** 中找到結尾為 `.hf.space` 的完整網址，這才是真正的 n8n 後台存取路徑，否則可能無法正常登入。
3.  完成 n8n 初始帳號設定，並記得至信箱完成驗證。

### 第四步：配置 Upstash Redis (選配)
1.  在 Upstash 創建一個 Redis 資料庫，區域同樣建議選擇 **California**。
2.  在 n8n 的 `Credentials` 中新增 Redis 設定：
    *   `Host`: Upstash 提供的 Endpoint。
    *   `Password`: Upstash 的密碼。
    *   `Port`: **6379**。
    *   **務必開啟 SSL (Enable SSL)**。
3.  在 AI Agent 節點中選擇 Redis Memory，可設定 `Session Time to Live` (秒) 實現長效記憶。

---

## 💡 進階：防止休眠解決方案 (Anti-Sleep Solution)

雖然 Hugging Face 提供兩天的活躍期，但若要確保工作流 24 小時不間斷，建議採用以下方法：

1.  **建立監控表**：在 Supabase 資料庫中建立一張名為 `health_check` 的空表。
2.  **設定 GitHub Actions**：在你的 GitHub 儲存庫設定一個 Workflow (.github/workflows/keep_alive.yml)，利用 Cron Job 定期執行（例如每 12 小時一次）。
3.  **定時打 API**：撰寫腳本定期發送 API 請求到 Supabase 的 REST 端點操作 `health_check` 表，藉此產生活動紀錄以維持 Hugging Face Space 的運行。

---

## 📅 維護與更新

*   **版本更新**：若需更新 n8n，可至 Hugging Face 的 `Settings` 執行 **Factory Rebuild**，這會拉取模板作者更新後的 Docker 映像檔（例如 1.94 版本或最新版）。
*   **備份建議**：雖然資料存於 Supabase，仍建議定期匯出工作流與憑證，以便未來需要遷移平台時可一鍵轉換。

---

## ⚠️ 注意事項

*   **Webhook 測試**：Hugging Face 部署已預設處理好 URL 問題，Webhook 會直接顯示 Space 的網址，不需另外使用 ngrok。
*   **休眠機制**：若 Space 超過兩天無活動進入休眠，只需再次瀏覽該網址即可喚醒。

---

本教學內容整理自 YouTube 頻道 [HC AI說人話](https://www.youtube.com/@HCAI-talk) 的分享。
```
