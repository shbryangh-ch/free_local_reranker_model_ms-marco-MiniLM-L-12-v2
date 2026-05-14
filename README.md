# free_local_reranker_model_ms-marco-MiniLM-L-12-v2

一個輕量、快速的本地 Reranker 服務，使用 **FlashRank** + **ms-marco-MiniLM-L-12-v2** 模型，專為 **自託管 Dify** 設計。

---

## ✨ 特點

- **極致輕量**：使用 FlashRank（ONNX 加速），模型僅約 100MB
- **快速部署**：FastAPI + Uvicorn，一鍵啟動
- **支援中文**：ms-marco-MiniLM-L-12-v2 對中英混合查詢有良好表現

---

## 📋 安裝

### 1. 複製專案
```bash
git clone https://github.com/你的帳號/flashrank-reranker.git
cd flashrank-reranker
```

### 2. 安裝依賴

```bash
pip install fastapi uvicorn flashrank python-multipart
```

---

## 🚀 快速啟動

```bash
uvicorn py檔案名稱:app --host 0.0.0.0 --port 8000 --reload
```

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


# 🔗 Dify / RAG 整合方式

可將此 API 接到 Dify 的模型供應商裡面的Local AI：
<img width="676" height="486" alt="圖片" src="https://github.com/user-attachments/assets/fa3a5ec1-6df9-48df-93ee-731c05260589" />

點按新增模型，點選rerank


<img width="407" height="290" alt="圖片" src="https://github.com/user-attachments/assets/551b0346-191e-4732-ac23-d4de1e84cb5a" />

* Server url: `http://your-ip:8010`

如果上面這個Server url出錯試試看這個
* Server url: `http://your-ip:8010/rerank`

如果你的自託管dify是裝在docker上面，your-ip用host.docker.internal

---

# 視窗標題（可選）

方便同時管理多個 server：

```bash
title BAAIbge-reranker
```
#通常是在開太多cmd視窗的時候可以用，使用後該視窗的標題會改成BAAIbge-reranker

---

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


## 📄 License

MIT License

---
