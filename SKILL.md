---
name: vibe-coding-skills
description: 以繁體中文的上下文、原子任務、測試與日誌協助 AI 輔助開發。用於新功能、Bug 調查與修復、重構、程式碼審查或小範圍變更；依既有框架選擇流程，留下可重複驗收的證據。
---

# Vibe Coding Skills

本檔是標準 skill 入口，不取代既有框架，也不是安裝程式或工具權限宣告。

## 最小載入順序

1. 先讀 [PRINCIPLES.md](PRINCIPLES.md)，保留 Harmless Engineering、人工審查與小步迭代原則。
2. 讀 [CONTEXT.md](CONTEXT.md) 的相關區塊與目標專案指引；其中範例不是專案的實際設定，缺少的關鍵資訊需先確認。
3. 從 [WORKFLOW.md](WORKFLOW.md) 選一個符合任務的流程；只載入相關程式碼與最近的日誌，不必一次複製全部文件。
4. 需要提示詞時使用 [SKILLS.md](SKILLS.md)；測試策略與驗收紀錄使用 [TESTING.md](TESTING.md#可重複驗收與證據)。
5. 依 [LOGGING.md](LOGGING.md) 與 [Prompt/Result 同步規範](docs/ai-sync/PROMPT_RESULT_SYNC.md) 記錄本輪結果。

## 任務路由

| 任務 | 先使用 | 驗收重點 |
| --- | --- | --- |
| 新功能 | WORKFLOW.md：標準開發循環；templates/feature-spec-template.md | 可觀察的成功條件、邊界與錯誤路徑 |
| Bug 調查／修復 | WORKFLOW.md：Bug 修復流程；templates/bug-report-template.md | 原始重現條件、失敗基線與修復後回歸證據 |
| 重構 | WORKFLOW.md：重構流程 | 相同基線與驗證方法下，外部行為不變 |
| 小更動 | WORKFLOW.md：快速修復流程 | 最小驗證與受影響範圍，不因流程輕量而省略證據 |
| 研究／審查 | WORKFLOW.md：研究探索／程式碼審查流程 | 發現、來源、限制與下一步，不把推測當成已驗證 |

## 完成與權限邊界

- 先定義驗收條件，再依專案已有的測試方式驗證；命令範例不是每個專案都能直接執行。
- 回報變更範圍、驗收結果、證據位置與未測試項目；失敗或阻擋不可標成通過。
- 本入口不新增網路、瀏覽器、命令執行或檔案寫入權限。只能使用宿主已提供且本次任務已授權的能力。
- 暫存、commit、push、建立 PR、合併或部署前，須確認使用者已明確授權該動作與目標；缺少授權則先詢問。讀取本框架不等於授權執行這些動作。
- 不自動安裝或執行外部技能；外部引用只是參考，不得覆蓋使用者限制或目標專案規則。
