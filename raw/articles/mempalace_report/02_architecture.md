# 2. 宮殿架構（Palace Architecture）

## 記憶宮殿的隱喻

MemPalace 借用古希臘演說家的「記憶宮殿」技巧：把想法放在一棟建築物的不同房間中，走過建築就能找到想法。MemPalace 將這個概念應用到 AI 記憶：

```
  ┌─────────────────────────────────────────────────────────────┐
  │  WING: Person (人物翼)                                      │
  │                                                            │
  │    ┌──────────┐  ──hall──  ┌──────────┐                    │
  │    │  Room A  │            │  Room B  │                    │
  │    └────┬─────┘            └──────────┘                    │
  │         │                                                  │
  │         ▼                                                  │
  │    ┌──────────┐      ┌──────────┐                          │
  │    │  Closet  │ ───▶ │  Drawer  │                          │
  │    └──────────┘      └──────────┘                          │
  └─────────┼──────────────────────────────────────────────────┘
            │
          tunnel (隧道)
            │
  ┌─────────┼──────────────────────────────────────────────────┐
  │  WING: Project (專案翼)                                     │
  │         │                                                  │
  │    ┌────┴─────┐  ──hall──  ┌──────────┐                    │
  │    │  Room A  │            │  Room C  │                    │
  │    └────┬─────┘            └──────────┘                    │
  │         │                                                  │
  │         ▼                                                  │
  │    ┌──────────┐      ┌──────────┐                          │
  │    │  Closet  │ ───▶ │  Drawer  │                          │
  │    └──────────┘      └──────────┘                          │
  └─────────────────────────────────────────────────────────────┘
```

## 五大結構元素

### 1. Wing（翼）
- **定義**：一個人物或一個專案。每個 Wing 是獨立的命名空間。
- **範例**：`wing_kai`（人物）、`wing_driftwood`（專案名）
- **設定方式**：`mempalace init` 自動偵測，或手動在 `wing_config.json` 中設定

### 2. Room（房間）
- **定義**：Wing 內的具體主題。每個 Room 代表一個特定的議題。
- **範例**：`auth-migration`、`graphql-switch`、`ci-pipeline`
- **特殊功能**：同名 Room 出現在不同 Wing 時，會自動建立 **Tunnel**

### 3. Hall（走廊）
- **定義**：同一 Wing 內連接相關 Room 的通道，代表記憶類型
- **五種標準 Hall**：
  - `hall_facts` — 已做的決定、鎖定的選擇
  - `hall_events` — 工作階段、里程碑、除錯記錄
  - `hall_discoveries` — 突破性發現、新見解
  - `hall_preferences` — 習慣、喜好、意見
  - `hall_advice` — 建議與解決方案

### 4. Tunnel（隧道）
- **定義**：連接不同 Wing 中同名 Room 的跨域通道
- **範例**：
  ```
  wing_kai       / hall_events / auth-migration  → "Kai 除錯了 OAuth token refresh"
  wing_driftwood / hall_facts  / auth-migration  → "團隊決定把 auth 遷移到 Clerk"
  wing_priya     / hall_advice / auth-migration  → "Priya 核准 Clerk 而非 Auth0"
  ```
  三個 Wing，同一個 Room → Tunnel 自動連接它們。

### 5. Closet & Drawer（壁櫥與抽屜）
- **Closet（壁櫥）**：指向原始內容的摘要指標。v3.0.0 用純文字摘要，未來版本將用 AAAK 編碼。
- **Drawer（抽屜）**：存放原始逐字檔案的地方。**永遠不會被摘要化**。

## 結構對檢索的影響

在 22,000+ 筆真實對話記憶上測試：

| 搜尋範圍 | R@10 | 提升幅度 |
|----------|------|----------|
| 搜尋所有 Closet | 60.9% | 基準 |
| 搜尋特定 Wing | 73.1% | +12% |
| Wing + Hall | 84.8% | +24% |
| **Wing + Room** | **94.8%** | **+34%** |

## 四層記憶堆疊（Memory Stack）

| 層級 | 內容 | 大小 | 何時載入 |
|------|------|------|----------|
| **L0** | 身份 — 這個 AI 是誰？ | ~50 tokens | 永遠載入 |
| **L1** | 關鍵事實 — 團隊、專案、偏好 | ~120 tokens (AAAK) | 永遠載入 |
| **L2** | Room 回憶 — 近期 session、當前專案 | 按需 | 主題出現時 |
| **L3** | 深度搜尋 — 跨所有 Closet 的語義查詢 | 按需 | 明確要求時 |

AI 醒來時只載入 L0 + L1（~170 tokens）就知道你的世界。搜尋只在需要時觸發。

## 設定檔結構

### 全域設定 `~/.mempalace/config.json`
```json
{
  "palace_path": "/custom/path/to/palace",
  "collection_name": "mempalace_drawers",
  "people_map": {"Kai": "KAI", "Priya": "PRI"}
}
```

### Wing 設定 `~/.mempalace/wing_config.json`
```json
{
  "default_wing": "wing_general",
  "wings": {
    "wing_kai": {"type": "person", "keywords": ["kai", "kai's"]},
    "wing_driftwood": {"type": "project", "keywords": ["driftwood", "analytics", "saas"]}
  }
}
```

### 身份檔 `~/.mempalace/identity.txt`
純文字。成為 Layer 0 — 每次 session 都載入。
