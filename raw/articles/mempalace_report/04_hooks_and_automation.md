# 4. 自動儲存 Hook 機制

## 概念

MemPalace 提供兩個 bash hook 腳本，讓 Claude Code（或 Codex CLI / Gemini CLI）在特定事件觸發時自動儲存記憶，無需手動操作。

## 兩種 Hook

| Hook | 觸發時機 | 行為 |
|------|----------|------|
| **Save Hook** (`mempal_save_hook.sh`) | 每 15 條人類訊息 | 阻斷 AI，要求它將重要主題/決策/引用存入 Palace |
| **PreCompact Hook** (`mempal_precompact_hook.sh`) | Context 壓縮前 | 緊急儲存 — 強制 AI 在失去詳細上下文前保存所有內容 |

## Save Hook 運作流程

```
User 送出訊息 → AI 回應 → Claude Code 觸發 Stop hook
                                    ↓
                            Hook 計算 JSONL transcript 中的人類訊息數
                                    ↓
                  ┌─── < 15 條 ──→ echo "{}" (讓 AI 正常停止)
                  │
                  └─── ≥ 15 條 ──→ {"decision": "block", "reason": "save..."}
                                          ↓
                                  AI 把記憶存入 Palace
                                          ↓
                                  AI 再次嘗試停止
                                          ↓
                                  stop_hook_active = true
                                          ↓
                                  Hook 看到 flag → echo "{}" (放行)
```

**防無限迴圈**：`stop_hook_active` 旗標確保只 block 一次。

## PreCompact Hook 運作流程

```
Context window 快滿 → Claude Code 觸發 PreCompact
                              ↓
                      Hook 永遠 block
                              ↓
                      AI 儲存所有內容
                              ↓
                      壓縮正常進行
```

不需要計數 — 壓縮前永遠值得儲存。

## 安裝方式

### Claude Code

在 `.claude/settings.local.json` 中加入：

```json
{
  "hooks": {
    "Stop": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "/absolute/path/to/hooks/mempal_save_hook.sh",
        "timeout": 30
      }]
    }],
    "PreCompact": [{
      "hooks": [{
        "type": "command",
        "command": "/absolute/path/to/hooks/mempal_precompact_hook.sh",
        "timeout": 30
      }]
    }]
  }
}
```

### Codex CLI

在 `.codex/hooks.json` 中加入類似設定。

### Gemini CLI

在 `~/.gemini/settings.json` 中使用 `PreCompress` 事件。

## 設定參數

在 `mempal_save_hook.sh` 中可調整：

| 參數 | 預設值 | 說明 |
|------|--------|------|
| `SAVE_INTERVAL` | 15 | 每幾條人類訊息觸發一次儲存 |
| `STATE_DIR` | `~/.mempalace/hook_state/` | Hook 狀態檔存放位置 |
| `MEMPAL_DIR` | 空白 | 設定後會自動執行 `mempalace mine <dir>` |

## 除錯

```bash
cat ~/.mempalace/hook_state/hook.log
```

範例輸出：
```
[14:30:15] Session abc123: 12 exchanges, 12 since last save
[14:35:22] Session abc123: 15 exchanges, 15 since last save
[14:35:22] TRIGGERING SAVE at exchange 15
[14:40:01] Session abc123: 18 exchanges, 3 since last save
```

## Save Hook 核心邏輯（技術細節）

1. 從 stdin 讀取 Claude Code 傳入的 JSON（含 `session_id`、`stop_hook_active`、`transcript_path`）
2. **安全處理**：用 `tr -cd 'a-zA-Z0-9_-'` 清理 session_id，防止路徑遍歷
3. 若 `stop_hook_active=true`，回傳 `{}`（放行）
4. 用內嵌 Python 腳本計算 JSONL transcript 中的 `role: user` 訊息數（跳過系統指令）
5. 比對上次儲存點（存在 `~/.mempalace/hook_state/{session_id}_last_save`）
6. 達到門檻時回傳 block JSON，觸發 AI 儲存

## 成本

**零額外 token**。Hook 是本地 bash 腳本，不呼叫任何 API。唯一的「成本」是 AI 花幾秒鐘整理記憶。
