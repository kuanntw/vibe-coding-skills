# TESTING.md — 測試策略

> **用途：** 定義測試分層、TDD 工作流、以及如何與 AI 協作產生與維護測試。
>
> **核心理念：** 測試是 Harmless Engineering 的第一道防線。沒有測試的程式碼 = 不安全的程式碼。

---

## 測試金字塔

```
           ┌──────┐
           │ E2E  │  ← 最少、最慢、最貴
           │ 10%  │     驗證完整使用者流程
          ┌┴──────┴┐
          │ 整合測試 │  ← 數量中等
          │   20%   │     驗證模組間的互動
         ┌┴────────┴┐
         │  單元測試  │  ← 最多、最快、最便宜
         │    70%    │     驗證單一函數/模組
         └──────────┘
```

| 層級 | 範圍 | 速度 | 維護成本 | 範例 |
|------|------|------|----------|------|
| 🔻 單元測試 | 單一函數/模組 | 毫秒 | 低 | `isValidEmail()` 回傳 false 當輸入不含 @ |
| 🔶 整合測試 | 多個模組互動 | 秒 | 中 | 註冊 API → 資料庫寫入 → 回傳 token |
| 🔺 E2E 測試 | 完整使用者流程 | 分鐘 | 高 | 使用者從註冊到完成首次登入 |

---

## 測試框架建議

### TypeScript / JavaScript

```
單元 + 整合測試：Vitest（推薦）或 Jest
E2E 測試：Playwright（推薦）或 Cypress
API 測試：Supertest
Mock：Vitest mock / MSW (Mock Service Worker)
```

### Python

```
單元 + 整合測試：pytest
E2E 測試：Playwright for Python
API 測試：httpx + pytest
Mock：unittest.mock / pytest-mock
```

---

## TDD 循環（Red → Green → Refactor）

### 完整的 TDD 工作流

```
Step 1: 🔴 RED
  寫一個會失敗的測試
  ↓
  跑測試 → 確認失敗（因為功能尚未實作）

Step 2: 🟢 GREEN
  寫最少程式碼讓測試通過
  ↓
  跑測試 → 確認通過（不追求優雅，只追求正確）

Step 3: 🔵 REFACTOR
  在測試的保護下重構程式碼
  ↓
  跑測試 → 確認仍然通過

Step 4: 回到 Step 1（下一個測試案例）
```

### 與 AI 協作的 TDD

```markdown
## Step 1: 請 AI 產生測試（RED）
「請根據以下需求，使用 Vitest 產生測試案例：

功能：建立一個驗證密碼強度的函數 validatePassword(password: string): ValidationResult

需求：
- 至少 8 個字元
- 至少包含 1 個大寫字母
- 至少包含 1 個數字
- 至少包含 1 個特殊字元
- 不能是常見密碼（如 password123）

測試需涵蓋：正常情況、邊界情況、錯誤情況」

## Step 2: 審查測試
確認測試是否合理、是否涵蓋需求

## Step 3: 請 AI 實作（GREEN）
「請實作 validatePassword 函數，使其通過上述測試」

## Step 4: 跑測試驗證
npm test -- validatePassword

## Step 5: 請 AI 重構（REFACTOR）
「validatePassword 運作正常，但程式碼可讀性可以提升。請重構，
保持所有測試通過。」
```

---

## 測試命名規範

### 格式

```
應該_[預期行為]_當_[條件]

或

[函數名稱]_[條件]_[預期結果]
```

### 範例

```typescript
// ✅ 好的測試命名 — 一看就懂在測什麼
describe('validatePassword', () => {
  it('應該回傳 valid: false 當密碼少於 8 個字元', () => { ... })
  it('應該回傳 valid: false 當密碼缺少大寫字母', () => { ... })
  it('應該回傳 valid: false 當密碼是常見密碼', () => { ... })
  it('應該回傳 valid: true 當密碼符合所有規則', () => { ... })
  it('應該回傳 valid: false 當輸入為空字串', () => { ... })
})

// ❌ 不好的測試命名 — 不知道在測什麼
it('test1', () => { ... })
it('works correctly', () => { ... })
```

---

## AI 生成測試的審查重點

AI 生成的測試需要人工確認以下事項：

```
□ 測試是否真的驗證了正確的行為？（不是假通過）
□ 邊界情況是否涵蓋？（null、空值、極值）
□ 是否有 false positive？（測試通過但實際上沒測到）
□ Mock 是否正確隔離了外部依賴？
□ 測試是否過度耦合實作細節？（重構時測試不該失敗）
□ 測試名稱是否清晰描述測試意圖？
```

---

## 測試覆蓋率目標

| 類型 | 最低覆蓋率 | 建議覆蓋率 |
|------|-----------|-----------|
| 核心商業邏輯 | 90% | 95%+ |
| 工具函數 | 80% | 90%+ |
| UI 元件 | 60% | 80%+ |
| API 路由 | 80% | 90%+ |
| 設定檔 | 不需要 | 不需要 |

> **不是追求 100% 覆蓋率，而是確保關鍵路徑有充分測試。**

---

## 何時寫測試

| 情境 | 測試策略 |
|------|----------|
| 🆕 新功能開發 | TDD：先寫測試，再寫實作 |
| 🐛 Bug 修復 | 先寫回歸測試（重現 Bug），再修復，確認測試通過 |
| 🔄 重構 | 確保既有測試全過，重構後再跑一次確認 |
| 📦 第三方整合 | 寫整合測試，Mock 外部服務 |
| 🔒 安全性修復 | 寫針對該漏洞的測試，確認修復有效 |

---

## 常用測試 Pattern

### Arrange → Act → Assert（AAA）

```typescript
it('應該回傳 user 當 id 存在時', async () => {
  // Arrange：準備測試資料
  const mockUser = { id: '1', name: 'Alice' }
  vi.spyOn(db, 'findUser').mockResolvedValue(mockUser)

  // Act：執行被測的動作
  const result = await getUserById('1')

  // Assert：驗證結果
  expect(result).toEqual(mockUser)
})
```

### Given → When → Then（BDD 風格）

```typescript
describe('購物車結帳', () => {
  it('應該計算正確的總金額，包含折扣', () => {
    // Given：購物車中有 2 件商品，使用者有 9 折優惠券
    const cart = new Cart([
      { name: '書', price: 300 },
      { name: '筆', price: 50 }
    ])
    cart.applyCoupon('SAVE10') // 9 折

    // When：結帳
    const total = cart.checkout()

    // Then：總金額應該是 (300+50)*0.9 = 315
    expect(total).toBe(315)
  })
})
```

### Mock 外部依賴

```typescript
// ✅ 正確：Mock 外部 API，測試自己的邏輯
vi.mock('./emailService', () => ({
  sendEmail: vi.fn().mockResolvedValue({ success: true })
}))

it('應該在註冊成功後發送驗證郵件', async () => {
  await registerUser({ email: 'test@example.com', password: 'Valid1!' })
  expect(sendEmail).toHaveBeenCalledWith(
    'test@example.com',
    expect.stringContaining('verify')
  )
})

// ❌ 錯誤：測試 mock 本身而不是實際邏輯
it('mock 應該回傳成功', async () => {
  const result = await sendEmail('test@example.com')
  expect(result.success).toBe(true) // 這只是在測 mock
})
```

---

## CI/CD 中的測試

```yaml
# GitHub Actions 範例
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm ci
      - run: npm run lint          # 1. Lint 檢查
      - run: npm run typecheck     # 2. 型別檢查
      - run: npm test              # 3. 單元 + 整合測試
      - run: npm run test:e2e      # 4. E2E 測試
```

---

## 測試維護規則

```
□ 每次新增功能 → 至少新增 1 個測試
□ 每次修復 Bug → 新增 1 個回歸測試
□ 每週檢查測試覆蓋率是否有下降
□ CI 中任何測試失敗 → 禁止 merge
□ 測試也是程式碼 → 也需要重構與維護
□ 測試不該比被測程式碼更難理解
```

---

> **記住：** 你今天寫的測試，是明天重構時的安全網，也是 AI 生成程式碼的第一道品質關卡。