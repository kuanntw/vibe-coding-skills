# CONTEXT.md — 專案上下文

> **用途：** 每次與 AI 開始對話時載入此文件，讓 AI 理解專案的技術背景、風格與約束。
>
> **使用方式：** 依照你的專案填寫下方各區塊。填完後，每次與 AI 對話時引用此文件。

---

## 專案資訊

```markdown
- 專案名稱：（例如：MySaaS）
- 一句話描述：（例如：一個協助小型團隊管理專案任務的 SaaS 平台）
- 目標使用者：（例如：5-50 人的軟體開發團隊）
```

### 核心邊界

```markdown
## 我們做什麼
- （例如：任務管理、團隊協作、時間追蹤）

## 我們不做什麼
- （例如：即時通訊、檔案儲存、薪資管理）
```

### 成功指標

```markdown
- （例如：使用者能在 3 分鐘內建立第一個專案）
- （例如：頁面載入時間 < 2 秒）
```

---

## 技術棧

### 前端

```markdown
- 框架：（例如：React 18 + TypeScript / Vue 3 / Next.js 14）
- UI 庫：（例如：Tailwind CSS / Chakra UI / Ant Design）
- 狀態管理：（例如：Zustand / Pinia / Redux Toolkit）
- 建置工具：（例如：Vite / Webpack / Turbopack）
```

### 後端

```markdown
- 語言/框架：（例如：Python 3.11 + FastAPI / Node.js + Express / Go + Gin）
- 資料庫：（例如：PostgreSQL 16 / MongoDB 7 / SQLite）
- ORM：（例如：Prisma / SQLAlchemy / Drizzle）
- 快取：（例如：Redis）
```

### 基礎設施

```markdown
- 部署平台：（例如：Vercel / AWS / Docker + K8s）
- CI/CD：（例如：GitHub Actions / GitLab CI）
- 監控：（例如：Sentry / Datadog / Grafana）
```

---

## 專案結構

```
專案根目錄/
├── src/                    # 原始碼
│   ├── components/         # 共用元件
│   ├── pages/              # 頁面（或 app/ for Next.js App Router）
│   ├── hooks/              # 自訂 Hooks / Composables
│   ├── services/           # API 服務層
│   ├── utils/              # 工具函數
│   ├── types/              # TypeScript 型別定義
│   └── styles/             # 全域樣式
├── tests/                  # 測試檔案
│   ├── unit/               # 單元測試
│   ├── integration/        # 整合測試
│   └── e2e/                # 端對端測試
├── prisma/                 # 資料庫 Schema（如果使用 Prisma）
├── public/                 # 靜態資源
├── docs/                   # 文件
└── logs/                   # 開發日誌
```

---

## 程式碼風格

### 命名規範

| 類型 | 規範 | 範例 |
|------|------|------|
| 檔案名 | kebab-case | `user-service.ts` |
| 變數名 | camelCase | `userName` |
| 函數名 | camelCase | `getUserById()` |
| 類別名 | PascalCase | `UserService` |
| 常數 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| 型別/介面 | PascalCase（不用 I 前綴） | `User`, `UserProfile` |
| 資料庫欄位 | snake_case | `created_at` |

### TypeScript 規範

```typescript
// ✅ 正確
function getUserById(id: string): Promise<User | null> {
  // ...
}

// ❌ 錯誤 — 使用 any
function getUserById(id: any): any {
  // ...
}

// ✅ 正確 — 使用 interface 定義物件形狀
interface CreateUserInput {
  name: string;
  email: string;
  age?: number;
}

// ❌ 避免 — 使用 inline type 定義大量參數
function createUser(name: string, email: string, age: number, role: string, ...) {
  // ...
}
```

### 錯誤處理規範

```typescript
// ✅ 正確 — 明確處理錯誤
try {
  const user = await userService.findById(id);
  if (!user) {
    throw new NotFoundError(`User ${id} not found`);
  }
  return user;
} catch (error) {
  logger.error('Failed to fetch user', { id, error });
  throw new ServiceError('Unable to fetch user', { cause: error });
}

// ❌ 錯誤 — 吞掉錯誤不處理
try {
  const user = await userService.findById(id);
  return user;
} catch (error) {
  // 什麼都不做
}
```

---

## AI 對話指引

### 每次對話的最少上下文

貼上以下內容到 AI 對話的開頭：

```markdown
我正在開發 {{專案名稱}}。

## 專案願景
{{貼上 CONTEXT.md 的專案資訊區塊}}

## 技術棧
{{貼上 CONTEXT.md 的技術棧區塊}}

## 當前任務
{{描述你今天要做的事}}
```

### 功能開發時的完整上下文

```markdown
我正在開發 {{專案名稱}}。

## 專案願景
{{CONTEXT.md → 專案資訊}}

## 技術棧
{{CONTEXT.md → 技術棧}}

## 相關程式碼風格
{{CONTEXT.md → 程式碼風格}}

## 相關檔案
{{列出會修改到的檔案路徑}}

## 最近的變更
{{貼上 logs/implementation-log.md 最近 2-3 筆記錄}}

## 任務
{{貼上 dev-tasks.md 中的任務}}
```

### 上下文維護規則

1. **每次技術棧變更** → 更新此文件的技術棧區塊
2. **每次架構變更** → 更新 LOGGING.md 的決策記錄 (ADR)
3. **每次風格決策** → 更新此文件的程式碼風格區塊
4. **新成員加入** → 請他們先閱讀此文件

---

## 當前狀態

請在此記錄當前專案的實際狀態（對於既有專案導入）：

```markdown
- 已實作功能：
- 開發中的功能：
- 已知問題：
- 技術債清單：
```

---

> **提示：** 此文件是 AI 的「專案記憶起點」。保持更新，AI 生成的程式碼品質會顯著提升。