# Number Monitor App (FastAPI + Supabase)

這是一個簡單的數字監控應用程式，包含使用者輸入頁面與管理後台。管理後台使用 Supabase Realtime 實現即時更新。

## 技術棧
- **後端**: Python (FastAPI)
- **資料庫**: Supabase (PostgreSQL + Realtime)
- **前端**: HTML, CSS, JavaScript (Vanilla)
- **部署**: Render

## 功能特點
- 使用者可提交整數數字。
- 管理員頁面 (`/admin`) 即時顯示最新 50 筆紀錄。
- 當數值 >= 55 時，自動標示為「危險」（紅色）。
- **即時更新**: 採用 **Supabase Realtime**，無需重新整理頁面。

---

## 本機執行步驟

1. **安裝依賴**:
   ```bash
   pip install -r requirements.txt
   ```

2. **設定環境變數**:
   複製 `.env.example` 為 `.env` 並填入您的 Supabase 資訊：
   - `SUPABASE_URL`
   - `SUPABASE_ANON_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY`

3. **啟動伺服器**:
   ```bash
   uvicorn app.main:app --reload
   ```
   開啟 [http://127.0.0.1:8000](http://127.0.0.1:8000) 即可使用。

---

## Supabase 設定步驟

1. **建立資料表**:
   在 Supabase SQL Editor 執行 `sql/init.sql` 的內容。

2. **啟用 Realtime**:
   - 方式一 (SQL): `ALTER PUBLICATION supabase_realtime ADD TABLE numbers;`
   - 方式二 (Dashboard): 進入 `Database` -> `Replication` -> 點擊 `supabase_realtime` 列的 `Source` 數量 -> 將 `numbers` 資料表勾選為 Enable。

3. **RLS 設定 (建議)**:
   請參考 `sql/init.sql` 中的 RLS 建議指令來保障資料安全。

---

## Render 部署步驟

1. 將此專案推送到您的 GitHub / GitLab。
2. 在 Render 建立新的 **Web Service** 並連結該專案。
3. Render 會自動偵測 `render.yaml` 並設定環境變數。
4. **手動設定環境變數**:
   在 Render Dashboard 的 Environment 頁面填入 `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`。

---

## 專案結構
```
.
├── app/
│   ├── main.py          # FastAPI 進入點
│   ├── supabase_client.py
│   └── templates/       # HTML 模板
├── sql/
│   └── init.sql         # 資料庫指令
├── .env.example
├── requirements.txt
├── render.yaml          # Render 設定
└── README.md
```

## 安全性備註
目前 `/admin` 頁面為簡化版，尚未加入登入驗證。正式上線建議加入 Supabase Auth 或 FastAPI OAuth2 保護管理員路由。
