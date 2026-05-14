# Technical Design Template

> **用途：** 定義功能的技術設計，作為實作時的架構藍圖。
>
> **使用方式：** 複製此模板到 `02-features/{{feature-name}}/tech-design.md`，填入內容。

---

## 功能名稱

{{功能名稱}}

---

## 架構概覽

```
{{用 ASCII art 或文字描述整體架構}}

例如：
Client → API Gateway → Auth Service → Database
                  ↘ Email Service
```

---

## 元件設計

### 元件 1：{{名稱}}

- **職責：** {{這個元件做什麼}}
- **輸入：** {{接收什麼資料}}
- **輸出：** {{產出什麼資料}}
- **依賴：** {{依賴哪些其他元件}}

### 元件 2：{{名稱}}

- **職責：**
- **輸入：**
- **輸出：**
- **依賴：**

---

## 資料模型

```{{語言}}
{{資料結構定義，例如 TypeScript interface / Prisma schema / SQL}}

interface User {
  id: string
  email: string
  name: string
  emailVerified: boolean
  createdAt: Date
}
```

---

## API 設計

### {{HTTP Method}} {{路徑}}

- **描述：** {{這個 API 做什麼}}
- **Request：**
```json
{
  "field": "type"
}
```
- **Response（成功）：**
```json
{
  "field": "type"
}
```
- **Response（錯誤）：**
```json
{
  "error": "message"
}
```

---

## 效能考量

- {{需要考量的效能瓶頸}}
- {{預期的負載}}

---

## 安全性考量

- {{認證需求}}
- {{授權需求}}
- {{資料保護需求}}

---

## 技術風險

| 風險 | 影響 | 緩解方案 |
|------|------|----------|
| {{風險 1}} | {{高/中/低}} | {{方案}} |