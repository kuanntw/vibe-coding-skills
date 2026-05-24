# PROMPT_RESULT_SYNC.md — Prompt/Result 文件同步規範

> **用途：** 確保每次下 Prompt 與每次產出結果後，都能即時更新對應文件，避免知識流失與脈絡中斷。

---

## 指定存放目錄（Required）

以下為**固定存放位置**，請勿任意變更：

- Prompt/Result 規範文件：`docs/ai-sync/PROMPT_RESULT_SYNC.md`
- 每日互動日誌：`logs/implementation-log.md`
- 可重用洞察：`logs/insights.md`
- 關鍵決策：`logs/decisions-log.md`

若需要新增更細分目錄（例如 `docs/ai-sync/archive/`），需在 PR 描述中說明目的與遷移方式。

---

## 核心原則

1. **每次下 Prompt 必記錄（先記錄再送出）**
2. **每次收到 Result 必更新（先整理再進下一輪）**
3. **沒有 Prompt ID 與 Result ID，不進下一輪互動**
4. **可重用內容必沉澱到主題文件，不只留在對話紀錄**

---

## 建議文件位置

- `logs/implementation-log.md`：當日 AI 協作主日誌（可追蹤 Session 脈絡）
- `logs/insights.md`：可重用洞察、經驗、提示語最佳化結論
- 任務文件（例如 `dev-tasks.md`、feature spec、tech design）：沉澱可落地結論

> 若專案後續新增 `docs/ai-log/`、`docs/prompts/`、`docs/outputs/`，可沿用同一套欄位與流程。

---

## 單輪標準流程（One Prompt Loop）

### Step 1：送出 Prompt 前（30 秒）

先在日誌新增一筆 `Prompt Entry`：

- `Prompt ID`（例如 `P-2026-05-24-01`）
- 任務目標
- 上下文來源（引用哪些檔案/決策）
- 限制條件（技術/時間/風格）
- Prompt 原文（可節錄）

### Step 2：收到 Result 後（1–3 分鐘）

在同一筆記錄追加 `Result Entry`：

- `Result ID`（例如 `R-2026-05-24-01`）
- 回覆摘要（3–5 行）
- 可用性評估：`✅ 可直接用` / `⚠️ 需調整` / `❌ 不可用`
- 問題點（事實錯誤、格式不符、測試不足等）
- 下一輪調整假設（下一個 prompt 要改什麼）

### Step 3：沉澱到對應文件（2 分鐘）

把可重用內容同步到對應檔案：

- 規格改動 → `feature spec` / `tech-design`
- 測試策略 → `test-plan`
- 實作結論 → `logs/implementation-log.md`
- 通用提示語技巧/踩坑 → `logs/insights.md`

---

## 日誌模板（可直接貼）

```markdown
### [P-2026-05-24-01] {{任務標題}}
- Time: {{HH:mm UTC}}
- Goal: {{這輪想解決什麼}}
- Context: {{引用文件/片段}}
- Constraints: {{限制}}
- Prompt:
  """
  {{prompt 內容}}
  """

#### [R-2026-05-24-01] Result
- Time: {{HH:mm UTC}}
- Summary: {{3-5 行摘要}}
- Usability: ✅ / ⚠️ / ❌
- Issues: {{問題點}}
- Next Prompt Hypothesis: {{下一輪如何調整}}
- Synced Files:
  - `path/to/file` — {{同步內容摘要}}
```

---

## 品質門檻（Definition of Done for AI 協作）

完成一次 AI 協作，需同時滿足：

- [ ] 已建立 Prompt Entry
- [ ] 已補上 Result Entry
- [ ] 已同步至少 1 個對應文件
- [ ] 下一步行動明確（可直接進入下一輪）

---

## 與既有 Workflow 整合方式

在 `WORKFLOW.md` 的「Step 4：與 AI 協作實作」與「Step 6：更新日誌」之間，強制插入以下檢查：

- [ ] 本輪 Prompt/Result 是否都已記錄？
- [ ] 可重用資訊是否已沉澱到對應文件？

若任一項為否，**不得進入 commit**。
