# GIS 可視化決策平台

透過可自由設定的可視化圖層幫助用戶做出更好的決策。前台供使用者切換查看 ArcGIS 圖層，後台供管理員進行圖層 CRUD 與權限管理。

## 架構

```
┌──────────────────────────────────────────────────────────────┐
│                           Browser                            │
│                                                              │
│          Vue 3 · ArcGIS Maps SDK · PrimeVue · Tailwind CSS   │
└─────────────────────────────┬────────────────────────────────┘
                              │
                              │ REST API (JWT)
                              ▼
┌──────────────────────────────────────────────────────────────┐
│                     FastAPI (Python 3.11)                    │
│                                                              │
│          JWT Auth · RBAC · SQLAlchemy 2.0 Async · Alembic    │
└─────────────────────────────┬────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────┐
│                         MySQL 8.0                            │
└──────────────────────────────────────────────────────────────┘
```

| 端       | 技術                                                                                         |
| -------- | -------------------------------------------------------------------------------------------- |
| Frontend | Vue 3 · TypeScript · Pinia · Vue Router · ArcGIS Maps SDK · PrimeVue 4 · Tailwind CSS · Vite |
| Backend  | FastAPI · SQLAlchemy 2.0 (async) · MySQL · Alembic · JWT (python-jose) · Pydantic v2         |
| DevOps   | Docker · Docker Compose · GitHub Actions                                                     |

## 功能

- **地圖瀏覽**：ArcGIS Feature / Tile Service 圖層切換、透明度調整、圖例面板
- **認證**：JWT Login + Refresh Token 自動輪換
- **RBAC**：Admin / User 兩角色，權限透過 FastAPI Depends 分層
- **圖層管理（Admin）**：新增 / 編輯 / 刪除 / 搜尋 / 分頁 / 拖曳排序
- **Legend 自動抓取**：新增/更新圖層時自動從 ArcGIS 服務取得 legend/renderer

## 快速啟動（Docker）

```bash
# 1. Clone 兩個 repo 到同一目錄
git clone https://github.com/babiysqaz/GIS-backend  gis-backend
git clone https://github.com/babiysqaz/GIS-frontend gis-frontend

# 2. 啟動所有服務（MySQL + Backend + Frontend）
docker compose up --build

# 3. 初始化範例資料（另開 terminal）
docker compose exec backend python seed.py
```

服務啟動後：

| 服務               | URL                                |
| ------------------ | ---------------------------------- |
| 前台地圖           | http://localhost:3000              |
| 後台管理           | http://localhost:3000/admin/layers |
| API 文件 (Swagger) | http://localhost:8000/docs         |
| API 文件 (ReDoc)   | http://localhost:8000/redoc        |

### Demo 帳號

| 帳號              | 密碼   | 角色                  |
| ----------------- | ------ | --------------------- |
| admin@example.com | 123456 | Admin（可 CRUD 圖層） |
| user@example.com  | 123456 | User（唯讀）          |

## 本機開發

詳細指令請見各 repo 的 README：

- [GIS-backend](https://github.com/babiysqaz/GIS-backend)
- [GIS-frontend](https://github.com/babiysqaz/GIS-frontend)
