# LOGGING.md — 開發日誌系統

> **用途：** 給 AI 跨 session 的「記憶」。沒有日誌 = AI 每次對話從零開始。
>
> **核心概念：** AI Coding Assistant 沒有跨對話的記憶。日誌系統讓你把每次開發的決策、實作、Bug 記錄下來，下次對話時載入給 AI，它就能「記得」之前發生的事。

---

## 為什麼需要日誌？

```
有日誌的開發流程：

Session 1: 建立了 User model       → 記錄在 implementation-log.md
Session 2: 載入日誌給 AI            → AI 知道 User model 已存在
Session 3: 載入日誌給 AI            → AI 知道之前做了什麼、為什麼這樣做

沒有日誌的開發流程：

Session 1: 建立了 User model
Session 2: AI 不知道有 User model  → 可能重複建立或做出衝突的決策
Session 3: AI 還是不知道           → 每次都要重新解釋專案背景
```

---

## 五種日誌

| 日誌 | 檔案 | 用途 | 更新頻率 |
|------|------|------|----------|
| 📝 實作記錄 | `logs/implementation-log.md` | 記錄每次開發 session 做了什麼 | 每次開發 session 結束 |
| 🏗️ 決策記錄 | `logs/decisions-log.md` | 架構決策記錄 (ADR) | 做出重大決策時 |
| 🐛 Bug 記錄 | `logs/bug-log.md` | Bug 追蹤與修復過程 | 發現/修復 Bug 時 |
| ✅ 驗證記錄 | `logs/validation-log.md` | 部署後的觀察與驗證 | 每次部署後 |
| 💡 洞察記錄 | `logs/insights.md` | 學到的經驗與最佳實踐 | 持續更新 |

---

## 📝 實作記錄（implementation-log.md）

### 為什麼這個最重要？

這是每日開發的日記。每次與 AI 協作完，記錄做了什麼、改了什麼檔案、為什麼這樣做。下次對話時貼給 AI，它就能接續之前的工作。

### 模板

```markdown
# Implementation Log

---

### {{YYYY-MM-DD}} | Session: {{任務標題}}

**任務：** {{TASK-XXX 或簡短描述}}
**耗時：** {{花費時間}}

#### 實作內容
- {{做了什麼 1}}
- {{做了什麼 2}}

#### 修改的檔案
| 檔案 | 變更 | 原因 |
|------|------|------|
| `src/services/user.ts` | 新增 `verifyEmail` 函數 | 實作 email 驗證流程 |
| `src/routes/auth.ts` | 新增 GET `/verify` 路由 | 處理驗證連結的點擊 |
| `tests/user.test.ts` | 新增 6 個測試案例 | 覆蓋驗證流程的各種情況 |

#### 遇到的問題
- {{問題描述}} → {{解決方案}}

#### 下次繼續
- {{需要接著做的事}}
- {{未完成的項目}}

#### AI 協作摘要
- AI 產生了 {{功能}} 的初始實作
- 人工調整了 {{部分}}（原因：{{簡述}}）
```

### 範例

```markdown
### 2026-05-14 | Session: Email 驗證流程

**任務：** TASK-012 實作 email 驗證
**耗時：** 2.5 小時

#### 實作內容
- 註冊成功後產生 24 小時有效的驗證 token
- 發送驗證郵件（使用 SendGrid）
- 驗證端點：GET /api/auth/verify?token=xxx
- 過期 token 可重新發送

#### 修改的檔案
| 檔案 | 變更 | 原因 |
|------|------|------|
| `src/services/email.ts` | 新增 `sendVerificationEmail()` | 發送驗證郵件 |
| `src/services/token.ts` | 新增 `generateVerificationToken()` | 產生有時效的 token |
| `src/routes/auth.ts` | 新增 verify 端點 | 處理使用者點擊驗證連結 |
| `tests/auth.test.ts` | 新增 8 個測試 | 覆蓋正常/過期/重送等情況 |

#### 遇到的問題
- SendGrid API key 需在環境變數設定 → 已加入 `.env.example`
- 開發環境無法實際發送郵件 → 使用 Mailhog 做本地測試

#### 下次繼續
- 前端：註冊成功後顯示「請檢查信箱」的提示頁面
- 前端：驗證成功/失敗的結果頁面

#### AI 協作摘要
- AI 產生了 token 生成邏輯和路由處理
- 人工調整了 token 過期策略（從 1 小時改為 24 小時）
- 郵件模板由人工撰寫（品牌風格考量）
```

---

## 🏗️ 決策記錄（decisions-log.md）

### 用途

記錄重要的架構決策（ADR — Architecture Decision Record），讓未來的自己（和 AI）理解「為什麼當初這樣選」。

### 模板

```markdown
# Decisions Log (ADR)

---

### ADR-{{編號}}: {{決策標題}}

**狀態：** 提議 | 已接受 | 已廢棄 | 已取代
**日期：** {{YYYY-MM-DD}}
**決策者：** {{姓名或角色}}

#### 背景
{{為什麼需要做這個決策？上下文是什麼？}}

#### 決策
{{我們決定做什麼？}}

#### 選項評估
| 選項 | 優點 | 缺點 | 結論 |
|------|------|------|------|
| 方案 A | ... | ... | ✅ 選擇 |
| 方案 B | ... | ... | ❌ 不採用 |
| 方案 C | ... | ... | ❌ 不採用 |

#### 影響
{{這個決策會影響什麼？}}

#### 風險
{{有什麼風險？如何緩解？}}
```

### 範例

```markdown
### ADR-001: 選擇 Prisma 作為 ORM

**狀態：** 已接受
**日期：** 2026-05-14

#### 背景
需要選擇一個 ORM 來操作 PostgreSQL 資料庫。團隊對 TypeScript 生態系較熟悉。

#### 決策
使用 Prisma 作為 ORM。

#### 選項評估
| 選項 | 優點 | 缺點 | 結論 |
|------|------|------|------|
| Prisma | 型別安全、自動 migration、IDE 支援好 | 較重、query 效能中等 | ✅ 選擇 |
| Drizzle | 輕量、效能好、SQL-like | 生態系較新、文件較少 | ❌ |
| TypeORM | 成熟、功能多 | 型別不安全、Active Record 模式複雜 | ❌ |

#### 影響
- 所有資料庫操作使用 Prisma Client
- Schema 定義在 `prisma/schema.prisma`
- Migration 使用 `prisma migrate`

#### 風險
- Prisma 在複雜查詢可能效能不如 raw SQL → 必要時使用 `$queryRaw`
```

---

## 🐛 Bug 記錄（bug-log.md）

### 用途

記錄所有發現的 Bug，包含重現步驟、根因、修復方案。幫助 AI 識別模式，避免重複錯誤。

### 模板

```markdown
# Bug Log

---

### BUG-{{編號}}: {{簡短描述}}

**狀態：** 🔴 待處理 | 🟡 調查中 | 🟢 已修復
**嚴重程度：** Critical | High | Medium | Low
**發現日期：** {{YYYY-MM-DD}}
**修復日期：** {{YYYY-MM-DD}}（如果已修復）

#### 重現步驟
1. {{步驟 1}}
2. {{步驟 2}}
3. {{步驟 3}}

**預期行為：** {{預期}}
**實際行為：** {{實際}}

#### 環境
- 瀏覽器/裝置：{{資訊}}
- 版本：{{版本號}}

#### 錯誤訊息
```
{{錯誤訊息}}
```

#### 根因
{{根本原因分析}}

#### 修復方案
{{修復的內容與方式}}

#### 預防措施
- 新增測試：{{測試名稱}}
- {{其他預防措施}}
```

---

## ✅ 驗證記錄（validation-log.md）

### 用途

部署到 production 後的觀察記錄。驗證功能是否如預期運作、使用者回饋、效能數據。

### 模板

```markdown
# Validation Log

---

### 部署 {{版本號}} — {{YYYY-MM-DD}}

#### 部署內容
- {{功能 1}}
- {{修復 1}}

#### 觀察結果
| 指標 | 預期 | 實際 | 狀態 |
|------|------|------|------|
| {{指標 1}} | {{值}} | {{值}} | ✅/⚠️/❌ |

#### 使用者回饋
- {{回饋}}

#### 需要跟進
- {{待辦事項}}
```

---

## 💡 洞察記錄（insights.md）

### 用途

記錄開發過程中的學習、發現、最佳實踐。隨著時間累積成團隊的知識庫。

### 模板

```markdown
# Insights

---

### {{標題}}（{{YYYY-MM-DD}}）

**類型：** 技術發現 | 流程改善 | AI 協作技巧 | 效能優化

#### 情境
{{在什麼情境下發現的？}}

#### 洞察
{{學到了什麼？}}

#### 行動
{{基於這個洞察，我們做了什麼改變？}}
```

---

## 如何將日誌載入 AI 對話

### 每次新對話的載入 Prompt：

```markdown
以下是上次對話以來的開發記錄：

## 近期實作
{{logs/implementation-log.md → 最近 3 筆記錄}}

## 重要決策
{{logs/decisions-log.md → 相關 ADR}}

## 已知 Bug
{{logs/bug-log.md → Open 狀態的 Bug}}

請根據以上記錄，接續目前的開發工作。
```

### 快速版本（時間有限時）：

```markdown
上次 session 摘要：
- 完成了 {{摘要}}
- 修改了 {{檔案}}
- 下次要做 {{事項}}

已知問題：{{Open Bug 數量}} 個
最新決策：{{最新 ADR}}
```

---

## 日誌維護規則

```
□ 每次開發 session 結束 → 更新 implementation-log.md
□ 每次做出架構決策 → 更新 decisions-log.md (ADR)
□ 每次發現/修復 Bug → 更新 bug-log.md
□ 每次部署後 24 小時內 → 更新 validation-log.md
□ 學到新東西 → 記錄到 insights.md
□ 每週回顧一次所有日誌，確認是否有遺漏
```

---

> **最重要的規則：** 不做日誌 = AI 每次從零開始。花 5 分鐘記錄，省下未來數小時的重複解釋時間。