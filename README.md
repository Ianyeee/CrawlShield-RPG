# 🎮 AI 網頁防禦卡牌遊戲 (CrawlShield RPG) - 專案開啟與運行指南

本指南將引導您如何啟動並運行 **CrawlShield RPG** 專案。本專案包含三個核心服務：
1. **Frontend**: React 前端遊戲畫面 (Vite)
2. **Backend**: FastAPI 遊戲引擎與 MCP SSE 伺服器
3. **Agent**: 串接 Gemini 3.5 Flash 的 AI 軍師特工

---

## 🛠️ 啟動方式一：使用 Docker Compose (推薦 🐳)

這是最簡單、快速的啟動方式。Docker Compose 會自動為您打包並執行所有服務，並處理好網路連線。

### 1. 前提條件
* 請確保本機已安裝 [Docker Desktop](https://www.docker.com/products/docker-desktop/)，且 Docker 正在運行。
* 已準備好您的 **Gemini API Key**。

### 2. 設定環境變數
在 `agent/.env` 檔案中填入您的 Gemini API Key：
```env
GEMINI_API_KEY=您的_GEMINI_API_KEY
MCP_SERVER_URL=http://backend:8000/mcp/sse
```
*(已在 `agent/.env` 設定預設的金鑰，您也可以視需要進行修改。)*

### 3. 一鍵啟動服務
開啟終端機（PowerShell 或 CMD），切換到專案根目錄（`0608`）下，執行以下指令：
```bash
docker compose up --build
```
*(或使用舊版指令 `docker-compose up --build`)*

### 4. 訪問連結
啟動成功後，您可以在瀏覽器中訪問以下網址：
* 🎮 **前端遊戲畫面**: [http://localhost:3000](http://localhost:3000)
* ⚙️ **後端 API 伺服器**: [http://localhost:8000](http://localhost:8000)
* 🧠 **AI 軍師服務**: [http://localhost:8001](http://localhost:8001)

若要停止服務，請在終端機按下 `Ctrl + C`，或執行 `docker compose down`。

---

## 💻 啟動方式二：本地手動分步啟動 (無 Docker 運作)

如果您不想使用 Docker，可以在本機手動啟動各個服務。

### ⚠️ 前提條件
* 安裝 **Python 3.11+** (並確保 `git` 執行檔已加入系統環境變數中，因為 `GitPython` 需要呼叫系統 `git`)。
* 安裝 **Node.js 20+** (包含 `npm`)。

---

### 第一步：啟動 Backend (FastAPI)
1. 開啟終端機並進入 `backend` 目錄：
   ```bash
   cd backend
   ```
2. （建議）建立並啟動 Python 虛擬環境：
   ```bash
   # Windows 系統
   python -m venv venv
   .\venv\Scripts\activate
   ```
3. 安裝依賴套件：
   ```bash
   pip install -r requirements.txt
   ```
4. 啟動 FastAPI 服務（連接埠設定為 `8000`）：
   ```bash
   uvicorn app.main:app --reload --port 8000
   ```
   * 後端 API 網址：[http://localhost:8000](http://localhost:8000)

---

### 第二步：啟動 Agent (AI 軍師)
1. 開啟另一個新的終端機，並進入 `agent` 目錄：
   ```bash
   cd agent
   ```
2. （建議）建立並啟動 Python 虛擬環境：
   ```bash
   # Windows 系統
   python -m venv venv
   .\venv\Scripts\activate
   ```
3. 安裝依賴套件：
   ```bash
   pip install -r requirements.txt
   ```
4. 確認 `agent/.env` 的 Gemini API 金鑰已設定：
   ```env
   GEMINI_API_KEY=您的_GEMINI_API_KEY
   MCP_SERVER_URL=http://localhost:8000/mcp/sse
   ```
5. 啟動 Agent 服務（連接埠設定為 `8001`）：
   ```bash
   uvicorn game_agent:app --reload --port 8001
   ```
   * Agent 網址：[http://localhost:8001](http://localhost:8001)

---

### 第三步：啟動 Frontend (React + Vite)
1. 開啟第三個終端機，並進入 `frontend` 目錄：
   ```bash
   cd frontend
   ```
2. 安裝 Node 套件依賴：
   ```bash
   npm install
   ```
3. 以開發模式運行前端：
   ```bash
   npm run dev
   ```
4. 在瀏覽器中打開終端機所顯示的開發網址（通常為 [http://localhost:5173](http://localhost:5173)）。

---

## 📂 專案重要目錄與檔案說明

* `backend/app/main.py`: 後端路由進入點（包含 `/api/game/*` 與 `/mcp/*`）。
* `backend/app/game.py`: 遊戲的核心邏輯與狀態計算。
* `backend/app/git_manager.py`: 負責將遊戲狀態以 Git Commit/Reset 進行版本控制。
* `agent/game_agent.py`: 定義 AI Agent 系統提示詞、呼叫 Gemini 3.5 Flash，並透過 MCP Tool 與遊戲互動。
* `frontend/src/`: 前端 React 原始碼與 UI 設計。
* `game_project_architecture.md`: 系統架構、核心遊戲規則與實作分工說明文件。
