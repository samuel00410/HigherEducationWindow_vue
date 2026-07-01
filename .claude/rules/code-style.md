# 程式碼風格規範

## 技術棧

- **Tailwind CSS v4**：樣式主力，utility class 優先
- **SCSS**：副線，僅用於 UDN 元件庫客製化、`:deep()` 覆寫

---

## Tailwind 風格

### 使用原則

- 版面（flex/grid）、間距（p/m/gap）、顏色、RWD 響應式 → 全用 Tailwind class
- 不要同時用 Tailwind 和 SCSS 寫同一段樣式，避免優先權衝突
- RWD 用 Tailwind 的前綴：`sm:` `md:` `lg:` `xl:`

### Tailwind v4 CSS 變數語法

**v4 使用 `()` 引用 CSS 變數，不是 `[var()]`：**

| 用途 | ❌ v3 / 錯誤寫法 | ✅ v4 正確寫法 |
| ---- | ---------------- | -------------- |
| 最大寬度 | `max-w-[var(--container-max)]` | `max-w-(--container-max)` |
| 字級 | `text-[length:var(--fs-h2)]` | `text-(--fs-h2)` |
| 圓角 | `rounded-[var(--radius-pill)]` | `rounded-(--radius-pill)` |
| 間距 | `px-[var(--space-3)]` | `px-(--space-3)` |

固定數值仍用 `[]`，例如 `max-w-[290px]`、`min-h-[560px]` → 這是正確的。

### CSS Cascade Layer 規則

**重要：`src/styles/base.css` 的所有全域樣式必須包在 `@layer base {}` 內。**

Tailwind v4 的 utility class 放在 `@layer utilities`，若 base.css 的樣式**沒有放進任何 layer**，優先順序會高於所有 Tailwind utilities，導致 class 失效（例如 `max-w-[290px]` 被 `img { max-width: 100% }` 蓋掉）。

```css
/* ✅ 正確 — 包在 @layer base 裡 */
@layer base {
  img, svg, video {
    max-width: 100%;
    height: auto;
  }
}

/* ❌ 錯誤 — 沒有 layer，會蓋掉 Tailwind utilities */
img, svg, video {
  max-width: 100%;
}
```

### 全域共用資源

- Design token 定義於 `src/styles/tokens.css`（CSS 自訂屬性，在 media query 內隨斷點更新）
- Token 值會隨斷點自動響應，不需要在 Tailwind class 加 `md:` 前綴，例如：
  - `text-(--fs-h2)` 在不同斷點自動變 28px / 36px / 44px
- 多值 token（如 `--radius-brand: 8px 8px 40px 8px`）無法用 `()` 語法，改用 inline style：
  - `style="border-radius: var(--radius-brand)"`

### 常用設計 Token

| Token | 值 | 用途 |
| ----- | -- | ---- |
| `--c-brand` | `#472785` | Header / 區塊G底 |
| `--c-brand-deep` | `#3c1375` | 深紫區塊 D/F |
| `--c-brand-tint` | `#f2eeff` | 淺紫卡底 |
| `--c-peach` | `#ffe7d6` | CTA 條底 |
| `--radius-brand` | `8px 8px 40px 8px` | 招牌不對稱圓角 |
| `--radius-block` | `80px` | 深紫大區塊圓角 |

---

## SCSS 風格

### 使用時機（嚴格限制）

1. **引入 UDN 共用元件庫**的 SCSS 變數或 mixin：
   ```scss
   @use '@udn-digital-center/common-components/styles/variables' as udn;
   ```
2. **`:deep()` 覆寫**元件內部樣式（只能在 `<style lang="scss" scoped>` 裡做）：
   ```scss
   :deep(.udn-component-inner) {
     color: var(--c-brand);
   }
   ```
3. 無法用 utility class 表達的複雜選擇器（如 `nth-child`、偽元素組合）

### 不該用 SCSS 的情境

- 版面、間距、顏色 → 改用 Tailwind
- 全域 reset / base → 放 `src/styles/base.css`
- Design token 定義 → 放 `src/styles/tokens.css`

### 檔案位置

| 檔案 | 用途 |
| ---- | ---- |
| `src/styles/all.css` | 主入口，import Tailwind + tokens + base |
| `src/styles/udn-overrides.scss` | UDN 元件庫全域樣式覆寫 |
| `src/components/**/*.vue` | 元件級 SCSS 用 `<style lang="scss" scoped>` |

### 命名

- SCSS 變數用 `$kebab-case`
- 避免深層巢狀（最多 3 層），優先用 Tailwind 的組合取代

---

## Vue SFC 慣例

- 頁面樣式：Tailwind class 寫在 template，`<style>` 僅放 `:deep()` 或特殊需求
- 元件內有 SCSS 需求時：`<style lang="scss" scoped>`
- 不需要 SCSS 的元件：`<style scoped>` 即可（不用 lang="scss"）
