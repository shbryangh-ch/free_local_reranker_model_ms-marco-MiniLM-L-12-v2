# free_local_reranker_model_ms-marco-MiniLM-L-12-v2

一個輕量、快速的本地 Reranker 服務，使用 **FlashRank** + **ms-marco-MiniLM-L-12-v2** 模型，專為 **自託管 Dify** 設計。

---

## ✨ 特點

- **極致輕量**：使用 FlashRank（ONNX 加速），模型僅約 100MB
- **相容 Dify**：與原 BGE reranker 輸出格式一致，可直接替換
- **快速部署**：FastAPI + Uvicorn，一鍵啟動
- **支援中文**：ms-marco-MiniLM-L-12-v2 對中英混合查詢有良好表現
- **本地運行**：無需 GPU，適合自託管環境

---

## 📋 安裝

### 1. 複製專案
```bash
git clone https://github.com/你的帳號/flashrank-reranker.git
cd flashrank-reranker
```

### 2. 安裝依賴
```bash
pip install -r requirements.txt
```

或手動安裝：
```bash
pip install fastapi uvicorn flashrank python-multipart
```

---

## 🚀 快速啟動

```bash
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```

服務將運行於：`http://localhost:8000`

---

## 📡 API 使用

### Rerank 接口

**POST** `/rerank` 或 `/api/rerank`

**Request Body:**
```json
{
  "query": "什麼是大型語言模型？",
  "documents": [
    "大型語言模型（LLM）是指參數量龐大的深度學習模型...",
    "Transformer 架構是目前主流的 LLM 基礎...",
    "另一段不相關的文字..."
  ]
}
```

**Response:**
```json
{
  "results": [
    {
      "index": 0,
      "relevance_score": 0.9876
    },
    {
      "index": 1,
      "relevance_score": 0.6543
    },
    {
      "index": 2,
      "relevance_score": 0.1234
    }
  ]
}
```

---

## 🔧 Dify 串接設定

在 Dify 的 **Reranker** 模組中設定：

- **Type**：`API`
- **Endpoint**：`http://你的伺服器IP:8000/rerank`
- **API Key**：可留空（本服務目前未實作認證）
- **Model Name**：可任意填寫（不影響）

---

## 📁 專案結構

```
.
├── app.py                 # 主程式
├── requirements.txt       # 依賴列表
├── cache/                 # 模型快取目錄（自動產生）
└── README.md
```

---

## ⚙️ 配置說明

可在 `app.py` 中調整的參數：

```python
ranker = Ranker(
    model_name="ms-marco-MiniLM-L-12-v2",
    cache_dir="./cache/flashrank",
    max_length=1024
)
```

---

## 📌 注意事項

- 第一次運行會自動下載模型（需網路）
- 建議分配至少 **2GB** 記憶體
- 生產環境建議使用 `gunicorn` + `uvicorn` 或 Docker 部署

---

## 🐳 Docker 部署（選用）

如需 Docker 版本，請告訴我，我可以幫你撰寫 `Dockerfile` 和 `docker-compose.yml`。

---

## 📄 License

MIT License

---
