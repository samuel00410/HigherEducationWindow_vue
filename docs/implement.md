# 高教視窗 — 首頁開發規格（Homepage Dev Spec）

> 給 Claude Code 後續分段實作用。**本文件只涵蓋首頁**。
> 範圍依 `page-goal.md`：Header / Hero / 主要內容卡片區 / 共用區 / Footer。
> 不實作：登入、後台、搜尋互動、複雜動畫、表單送出。
>
> 資料來源：`.fig`（`1920` frame node `81:70024`、`320-s22` node `81:48060`）為主，
> `page-goal.md` 為補充確認。

---

## 技術棧確認

| 層級     | 技術                                    | 說明                                                       |
| -------- | --------------------------------------- | ---------------------------------------------------------- |
| 框架     | Vue 3 + Vite                            | SFC 元件開發                                               |
| 樣式主力 | **Tailwind CSS v4**                     | 版面、間距、顏色、RWD utility class                        |
| 樣式副線 | **SCSS**                                | 僅用於 UDN 共用元件庫覆寫、`:deep()` 選擇器、變數 override |
| 共用元件 | `@udn-digital-center/common-components` | 需 SCSS 才能客製化內部樣式                                 |

**使用邊界原則：**

- 能用 Tailwind class 的一律用 Tailwind，不要在 SCSS 重複寫相同邏輯
- SCSS 僅在以下情境使用：引入 UDN package 的 SCSS 變數、`:deep()` 覆寫元件內部樣式、無法用 utility class 表達的複雜選擇器
- Design token（顏色、字級、spacing）定義於 `src/styles/tokens.css`，SCSS 可透過 `@use`/`@forward` 引用

**Tailwind v4 重要語法差異：**

| 用途         | ❌ v3 寫法（勿用）             | ✅ v4 正確寫法            |
| ------------ | ------------------------------ | ------------------------- |
| CSS 變數引用 | `max-w-[var(--container-max)]` | `max-w-(--container-max)` |
| 字級 token   | `text-[length:var(--fs-h2)]`   | `text-(--fs-h2)`          |
| 圓角 token   | `rounded-[var(--radius-pill)]` | `rounded-(--radius-pill)` |
| 間距 token   | `px-[var(--space-3)]`          | `px-(--space-3)`          |

固定數值仍用 `[]`，如 `max-w-[290px]`、`min-h-[560px]`。

**CSS Cascade Layer 規則（`src/styles/base.css` 必須遵守）：**

`base.css` 的所有樣式必須包在 `@layer base {}` 裡。沒有 layer 的 CSS 優先順序高於 `@layer utilities`，會導致 Tailwind utility class 失效。詳見 `.claude/rules/code-style.md`。

---

## 1. 首頁 Frame 對應（Breakpoints）

### 1-A. Figma Frame 命名規則

`.fig` 內 frame 的命名有三種型態：

| 型態         | 範例                 | 說明                                         |
| ------------ | -------------------- | -------------------------------------------- |
| `{寬度}`     | `1920`、`375`        | 大多是空 stub 或僅含部分內容                 |
| `{寬度}-s2`  | `320-s2`、`744-s2`   | 空 stub（index.jsx < 100b），不含實際內容    |
| `{寬度}-s22` | `320-s22`、`744-s22` | **完整 frame**，與 1920 對等，實作以這組為準 |

> `320`、`375`、`744`、`1024`、`1440` 主 frame 的 index.jsx 均為空 stub（86–95 bytes），內容在 `-s22` 裡。
> **實作時請以 `1920`（node `81:70024`）和 `320-s22`（node `81:48060`）為雙主軸對照。**

### 1-B. Breakpoint 對應表

| 角色         | 設計寬度 | 完整 fig frame               | CSS min-width 斷點 |
| ------------ | -------- | ---------------------------- | ------------------ |
| mobile-sm    | 320px    | `320-s22`（node `81:48060`） | — (base)           |
| mobile       | 375px    | `375-s22`                    | `375px`            |
| tablet       | 744px    | `744-s22`                    | `744px`            |
| desktop-sm   | 1024px   | `1024-s22`                   | `1024px`           |
| desktop      | 1280px   | `1280-s22`                   | `1280px`           |
| desktop-lg   | 1440px   | `1440-s22`                   | `1440px`           |
| wide desktop | 1920px   | `1920`（node `81:70024`）    | `1920px`           |

> 本期以 **320 / 744 / 1024 / 1440 / 1920** 五個斷點為主要切版目標；375 和 1280 視內容撐開情況決定是否補個別 override。

建議 CSS token：

```css
:root {
  --bp-sm: 375px;
  --bp-md: 744px;
  --bp-lg: 1024px;
  --bp-xl: 1440px;
  --bp-2xl: 1920px;
}
```

---

## 2. 主要 Section 拆解（由上而下）

> 以 fig `1920` frame 為結構依據，`320-s22` 對應差異標注於各區塊。

| #   | Section                      | 內容重點                                                                                        | 桌面背景                                          | 行動背景                                          |
| --- | ---------------------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| A   | **Header bar**               | 左：聯合報 logo +「數位版」；右：膠囊外框按鈕「訂閱享首年85折」                                 | `#472785` / 高 60px；**絕對定位疊在 Hero 上**     | 同                                                |
| B   | **Hero / section01**         | 全幅畢業生背景圖；置中「高教視窗」標題鎖定圖；副標文字；下方 feature pill 外框容器（3 個 pill） | 背景照片 `center/cover`；hero 高 **1080px**       | hero 高 **568px**                                 |
| C   | **3大高教議題**              | 主標＋副標；議題卡容器（3卡＋分隔線）；底部桃色 CTA 條                                          | 白色 `#ffffff`                                    | **品牌紫 `#472785`** ⚠️                           |
| D   | **你其實比想像中更需要訂閱** | 主標＋副標；4 個 answer 卡                                                                      | 深紫 `#3C1375` / `border-radius: 80px`            | 深紫 `#3C1375` / `border-radius: 24px`            |
| E   | **口碑陣容**                 | 主標＋副標；2 張記者卡（橫排）                                                                  | 白色；`padding: 40px 48px 64px`                   | 白色；`padding: 32px 16px 40px`                   |
| F   | **限時優惠**                 | 主標；2 個 discount 卡；CTA 按鈕＋服務電話                                                      | 深紫 `#3C1375` / `border-radius: 80px 80px 0 0`   | 深紫 `#3C1375` / `border-radius: 24px 24px 0 0`   |
| G   | **他網 vs 聯合報數位版**     | 主標＋副標；6 張比較卡（橫排）                                                                  | 品牌紫 `#472785` / `border-radius: 0 0 80px 80px` | 品牌紫 `#472785` / `border-radius: 0 0 24px 24px` |
| H   | **國內外大獎肯定**           | 主標＋副標；3 張獎項卡                                                                          | 白色                                              | 白色                                              |
| I   | **Footer-information**       | 連結 / 聯絡資訊（共用元件）                                                                     | —                                                 | —                                                 |
| J   | **Educate Footer**           | 版權列（共用元件）                                                                              | —                                                 | —                                                 |

### Section 高度參考（fig 實測）

| Section       | 1920 高度 | 320-s22 高度 |
| ------------- | --------- | ------------ |
| A Header      | 60px      | 60px         |
| B Hero        | 1080px    | 568px        |
| C Issues      | 844px     | 964px        |
| D Answer      | 708px     | 1021px       |
| E Authors     | 418px     | 523px        |
| F Discount    | 953px     | 1095px       |
| G Compare     | 752px     | —            |
| H Awards      | 712px     | —            |
| I Footer-info | 790px     | —            |
| J Footer-bar  | 169px     | —            |

---

## 3. Section 細節差異：桌面 vs 行動

> 這節補充 §2 表格無法細表的「斷點間結構差異」，每個 section 開發時必須對照。

### A. Header Bar

| 項目     | 1920（桌面）                                     | 320-s22（行動）             |
| -------- | ------------------------------------------------ | --------------------------- |
| 定位方式 | `position: absolute; top: 1px` 疊在 hero 上      | 同                          |
| 高度     | 60px                                             | 60px                        |
| padding  | `12px 15px`                                      | 同（推測）                  |
| 訂閱按鈕 | 寬 158px、高 50px、`border-radius: 50px`（膠囊） | 同結構，尺寸待 744-s22 確認 |
| 字體     | Noto Serif TC Regular 16px                       | 同                          |

### B. Hero / Section01

| 項目              | 1920                                                                      | 320-s22                                                                  |
| ----------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 高度              | 1080px                                                                    | 568px                                                                    |
| inner container   | `max-width: 1100px`、`padding-bottom: 40px`                               | full width、`padding: 0 0 24px`                                          |
| logo 鎖定圖寬     | 581px                                                                     | 290px                                                                    |
| 副標字級          | **30px** Bold，`color: rgb(255,231,214)`                                  | **18px** Bold，同色                                                      |
| Feature pill 容器 | `border-radius: 80px`、flex **row**、`padding: 12px 80px`、`height: 96px` | `border-radius: 24px`、flex **column**、`padding: 16px`、`height: 218px` |
| Feature pill 高   | 72px × 3（橫排）                                                          | 54px × 3（直排）                                                         |
| pill gap          | flex justify-content: space-between                                       | `gap: 12px`                                                              |

### C. 3大高教議題

| 項目                     | 1920                               | 320-s22                                             |
| ------------------------ | ---------------------------------- | --------------------------------------------------- |
| **區塊背景**             | **白色 `#ffffff`**                 | **品牌紫 `#472785`** ⚠️ 重要差異                    |
| 區塊 padding             | `80px 48px`                        | `40px 16px`                                         |
| 區塊 gap                 | `48px`（內容元素間）               | `24px`                                              |
| 議題卡容器背景           | `#F2EEFF`（淺紫）                  | `#3C1375`（深紫）                                   |
| 議題卡容器 border-radius | `8px 8px 40px 8px`                 | `8px 8px 24px 8px`                                  |
| 議題卡排列               | flex **row**，3 欄，Line3 垂直分隔 | flex **column**，3 列，Line3 水平分隔（rotate 90°） |
| CTA 桃色條 border-radius | `8px 8px 40px 8px`                 | `8px 8px 24px 8px`                                  |
| CTA 條 padding           | `24px 40px`                        | `24px 16px`                                         |
| CTA 條 flex 方向         | **row**（文字左、按鈕右）          | **column**（文字上、按鈕下）                        |
| 訂閱文字字級             | **30px** Bold                      | **20px** Bold                                       |
| 訂閱按鈕尺寸             | 寬 282px × 高 65px                 | 全寬 × 高 56px                                      |

### D. 你其實比想像中更需要訂閱

| 項目               | 1920                                                                             | 320-s22                          |
| ------------------ | -------------------------------------------------------------------------------- | -------------------------------- |
| 背景色             | `#3C1375`                                                                        | `#3C1375`                        |
| border-radius      | **80px**                                                                         | **24px**                         |
| padding            | `80px 48px`                                                                      | `40px 16px`                      |
| answer-ta-box 排列 | flex **row**，`flex-wrap: wrap`，`align-content: space-between`，gap: 56px → 2×2 | flex **column**，gap: 32px → 1×4 |
| section heading 高 | 110px                                                                            | 158px                            |
| inner 容器 gap     | 24px                                                                             | 16px                             |

### E. 口碑陣容

| 項目       | 1920                    | 320-s22                    |
| ---------- | ----------------------- | -------------------------- |
| padding    | `40px 48px 64px 48px`   | `32px 16px 40px 16px`      |
| 記者卡排列 | flex **row**，gap: 56px | flex **column**，gap: 32px |
| 記者卡高   | 180px × 2               | 156px × 2                  |

### F. 限時優惠

| 項目            | 1920                                       | 320-s22                    |
| --------------- | ------------------------------------------ | -------------------------- |
| border-radius   | `80px 80px 0 0`（上圓角）                  | `24px 24px 0 0`            |
| padding         | `80px 48px`                                | `40px 16px`                |
| discount 卡排列 | flex **row**，`padding: 0 56px`，gap: 40px | flex **column**，gap: 40px |
| discount 卡高   | 531px / 555px（兩卡不等高）                | 332px / 357px              |

### G. 比較卡（他網 vs 聯合報）

| 項目          | 1920                                                   | 320（推測）                                |
| ------------- | ------------------------------------------------------ | ------------------------------------------ |
| border-radius | `0 0 80px 80px`（下圓角）                              | `0 0 24px 24px`                            |
| padding       | `80px 48px`                                            | `40px 16px`                                |
| 比較卡排列    | flex **row**，6 卡並排，`max-width: 1800px`，gap: 24px | 橫向滑動（overflow-x: auto + scroll-snap） |
| 卡片高        | 458px                                                  | 滑動，高度待確認                           |

### H. 國內外大獎

| 項目       | 1920                                               | 320（推測）                      |
| ---------- | -------------------------------------------------- | -------------------------------- |
| padding    | `80px 48px`                                        | `40px 16px`                      |
| 獎項卡排列 | flex **row**，3 欄，`max-width: 1100px`，gap: 24px | flex **column** 或 grid auto-fit |

---

## 4. HTML 語意結構建議

純 HTML，BEM-ish class 命名（`block__element--modifier`）。骨架：

```html
<header class="site-header">
  <!-- logo + cta-button -->
</header>

<main>
  <section class="hero" aria-label="高教視窗">
    <!-- hero__bg（背景圖，CSS background）-->
    <div class="hero__inner">
      <img class="hero__logo-lockup" src="educate-logo-title.svg" alt="高教視窗" />
      <p class="hero__subtitle">升等、教育變革…</p>
      <ul class="feature-pills">
        <li class="feature-pill">…</li>
        <!-- × 3 -->
      </ul>
    </div>
  </section>

  <section class="issues">
    <div class="container">
      <header class="section-heading">
        <h2>…</h2>
        <p>…</p>
      </header>
      <div class="issues__card-container">
        <article class="issue-card">…</article>
        <!-- × 3 + Line3 分隔 -->
      </div>
      <aside class="issues__cta">
        <div class="issues__cta-text">…</div>
        <a class="cta-button" href="#">訂閱首年85折</a>
      </aside>
    </div>
  </section>

  <section class="need">
    <div class="container">
      <header class="section-heading">…</header>
      <ul class="answer-grid">
        <li class="answer-card">…</li>
        <!-- × 4 -->
      </ul>
    </div>
  </section>

  <section class="authors">
    <div class="container">
      <header class="section-heading">…</header>
      <div class="authors__grid">
        <article class="author-card">…</article>
        <!-- × 2 -->
      </div>
    </div>
  </section>

  <section class="discount">
    <div class="container">
      <header class="section-heading">…</header>
      <div class="discount__grid">
        <article class="discount-card">…</article>
        <!-- × 2 -->
      </div>
      <div class="discount__cta">
        <a class="cta-button" href="#">…</a>
        <p class="discount__phone">…</p>
      </div>
    </div>
  </section>

  <section class="compare">
    <div class="container container--wide">
      <header class="section-heading">…</header>
      <div class="compare__track">
        <article class="compare-card">…</article>
        <!-- × 6 -->
      </div>
    </div>
  </section>

  <section class="awards">
    <div class="container">
      <header class="section-heading">…</header>
      <div class="awards__grid">
        <article class="award-card">…</article>
        <!-- × 3 -->
      </div>
    </div>
  </section>
</main>

<footer class="site-footer">
  <div class="footer-info">…</div>
  <!-- I: footer-information 元件 -->
  <div class="footer-bar">…</div>
  <!-- J: educate-footer 元件 -->
</footer>
```

語意要點：

- Header 是 `position: sticky`（或 `fixed`）疊在 hero 上，背景紫色。
- 每個 section 用 `<h2>` 當主標（頁面唯一 `<h1>` 給 Hero logo 鎖定圖的 `alt` 或 `aria-label`）。
- 卡片用 `<article>`；純裝飾圖示 `alt=""` / `aria-hidden="true"`。
- feature pill、answer 卡清單用 `<ul><li>`。
- CTA 是連結就用 `<a>`，非 `<button>`（非送出表單）。

---

## 5. CSS Layout 策略

- **Tailwind v4 為主**，utility class 處理版面、間距、RWD；CSS 自訂屬性（tokens）補充品牌 token。
- **避免固定高度**：用 `padding` 撐高、`min-height` 例外才用；卡片等高交給 flex `align-items: stretch` 拉伸。

### 置中容器

```css
.container {
  width: 100%;
  max-width: var(--container-max); /* 1100px */
  margin-inline: auto;
  padding-inline: var(--gutter);
  box-sizing: border-box;
}
.container--wide {
  max-width: var(--container-wide); /* 1800px，compare 區 */
}
```

### 各 Section 版面

| Section            | 桌面版面                                  | 行動版面                                   |
| ------------------ | ----------------------------------------- | ------------------------------------------ |
| Hero feature pills | flex row，justify-content: space-between  | flex column，gap: 12px                     |
| C 議題卡           | flex row，3 欄 + Line3 垂直分隔           | flex column，3 列 + Line3 水平分隔（旋轉） |
| D answer 卡        | flex row + wrap，gap: 56px（→ 2×2）       | flex column，gap: 32px                     |
| E 記者卡           | flex row，gap: 56px                       | flex column，gap: 32px                     |
| F discount 卡      | flex row，gap: 40px，padding-inline: 56px | flex column，gap: 40px                     |
| G 比較卡           | flex row，6 卡並排，gap: 24px             | overflow-x: auto + scroll-snap             |
| H 獎項卡           | flex row，3 欄，gap: 24px                 | flex column 或 grid                        |

### 比較區（G）橫向滑動

```css
.compare__track {
  display: flex;
  gap: 24px;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
}
.compare-card {
  flex-shrink: 0;
  scroll-snap-align: start;
}
@media (min-width: 1024px) {
  .compare__track {
    overflow-x: visible;
    flex-wrap: nowrap;
    justify-content: center;
  }
  .compare-card {
    flex: 1;
  }
}
```

---

## 6. RWD 對應方式

採 **mobile-first**，min-width media query 往上加。

### 6-A. 斷點變化總表

| 區塊                    | 320（base）        | 744px      | 1024px         | 1440px+            |
| ----------------------- | ------------------ | ---------- | -------------- | ------------------ |
| Header                  | sticky；全寬       | 同         | 同             | 同                 |
| Hero 高                 | 568px              | 推測 760px | 推測 900px     | 1080px             |
| Hero pills              | flex column        | flex row？ | flex row       | flex row           |
| Hero 副標字級           | 18px               | —          | 24px？         | 30px               |
| C 區塊背景              | **`#472785`**      | —          | —              | **`#ffffff`**      |
| C 議題卡排列            | flex column        | —          | flex row       | flex row           |
| C CTA 條方向            | flex column        | —          | flex row       | flex row           |
| D answer 卡             | flex column 1×4    | —          | flex row 2×2   | flex row 2×2       |
| E 記者卡                | flex column        | —          | flex row       | flex row           |
| F discount 卡           | flex column        | —          | flex row       | flex row           |
| G 比較卡                | scroll             | scroll     | scroll or wrap | 6 並排             |
| H 獎項卡                | flex column        | —          | flex row 3欄   | flex row 3欄       |
| 區塊 border-radius      | **24px**           | —          | —              | **80px**           |
| 非對稱圓角              | `8px 8px 24px 8px` | —          | —              | `8px 8px 40px 8px` |
| 區塊 padding-block      | 40px               | 60px       | 60px           | 80px               |
| gutter (padding-inline) | 16px               | 24px       | 40px           | 48px               |
| 卡片 gap                | 16–32px            | —          | 40–56px        | 40–56px            |

> ⚠️ `744px` 和 `1280px` 的 `-s22` frame 尚未細讀，表格中「—」為推估，實作前需補確認。

### 6-B. Section Padding Token（響應式）

```css
:root {
  --section-pad-y: 40px;
  --gutter: 16px;
  --radius-block: 24px;
  --radius-brand: 8px 8px 24px 8px;
  --card-gap: 16px;
}
@media (min-width: 744px) {
  :root {
    --section-pad-y: 60px;
    --gutter: 24px;
  }
}
@media (min-width: 1024px) {
  :root {
    --gutter: 40px;
    --card-gap: 40px;
  }
}
@media (min-width: 1440px) {
  :root {
    --section-pad-y: 80px;
    --gutter: 48px;
    --radius-block: 80px;
    --radius-brand: 8px 8px 40px 8px;
    --card-gap: 56px;
  }
}
```

---

## 7. Design Tokens

> 以下 hex 由 `.fig` METADATA `rgb()` 值直接轉換，不是近似。

### 顏色

```css
:root {
  /* 品牌紫 */
  --c-brand: #472785; /* rgb(71,39,133)   header / C(桌面)側邊背景 / section G */
  --c-brand-deep: #3c1375; /* rgb(60,19,117)   section D / F / C 議題卡容器(行動) */
  --c-brand-dark: #220c49; /* rgb(34,12,73)    最深紫，hover 或 overlay */
  --c-brand-tint: #f2eeff; /* rgb(242,238,255) C 議題卡容器(桌面) */
  --c-violet-mid: #6452ac; /* rgb(100,82,172)  次要紫 / 圖示 */

  /* 桃 / 米 */
  --c-peach: #ffe7d6; /* rgb(255,231,214) C CTA 底 / Hero 副標文字色 */
  --c-cream: #fff5ab; /* rgb(255,245,171) 強調黃（優惠標籤） */
  --c-cream-soft: #ffef9e; /* rgb(255,239,158) 次強調黃 */

  /* 紅 */
  --c-accent-red: #cb2529; /* rgb(203,37,41)   聯合報 logo / 「獨」標 */
  --c-red-mid: #de4447; /* rgb(222,68,71)   較亮的紅（部分 icon） */

  /* 中性 */
  --c-text: #232323; /* rgb(35,35,35)    主文 */
  --c-text-muted: #67645c; /* rgb(103,100,92)  次要文字 */
  --c-text-sub: #585856; /* rgb(88,88,86)    說明文字 */
  --c-white: #ffffff;
  --c-line: #d9d9d9; /* rgb(217,217,217) 分隔線 */
}
```

### 字體

```css
:root {
  --font-sans: 'Noto Sans TC', system-ui, 'Helvetica Neue', Arial, sans-serif;
  --font-serif: 'Noto Serif TC', serif;
}
```

字體使用規則：

| 用途                        | 字體          | 字重        |
| --------------------------- | ------------- | ----------- |
| 主文、說明、標籤            | Noto Sans TC  | 400 Regular |
| 強調、卡片標題、副標        | Noto Sans TC  | 700 Bold    |
| 區塊主標（h2）              | Noto Serif TC | 700 Bold    |
| 訂閱按鈕文字（桌面 header） | Noto Serif TC | 400 Regular |

> fig 原稿使用 FOT-Chiaro Std B（191 次）、zihunbiantaoti（84次）、Source Han Sans TC Heavy 等商用字型，**無法於 web 直接引用**。
> 替代方案：`--font-serif` 用 Noto Serif TC 替代；「高教視窗」標題鎖定圖維持 SVG 圖片（`educate-logo-title.svg`）。

### 字級（desktop 基準）

```css
:root {
  --fs-h2: 44px; /* 區塊 h2，Noto Serif TC Bold */
  --fs-h2-sub: 24px; /* 區塊副標 */
  --fs-hero-sub: 30px; /* Hero 副標（桌面） */
  --fs-card-lg: 30px; /* 大卡標題（CTA 條文字） */
  --fs-card: 24px; /* 卡片標題 */
  --fs-body: 20px; /* 主內文 */
  --fs-body-sm: 18px; /* 次要內文 */
  --fs-small: 16px; /* 標籤、按鈕、說明 */
}
```

行動版主要差異：

| token           | 桌面 | 行動（320）                  |
| --------------- | ---- | ---------------------------- |
| `--fs-h2`       | 44px | ~28px（待 320-s22 確認）     |
| `--fs-hero-sub` | 30px | **18px**（fig 確認）         |
| `--fs-card-lg`  | 30px | **20px**（fig 確認：CTA 條） |

### Spacing（8px scale）

```css
:root {
  --space-1: 8px;
  --space-2: 12px;
  --space-3: 16px;
  --space-4: 24px;
  --space-5: 32px;
  --space-6: 40px;
  --space-7: 48px;
  --space-8: 56px;
  --space-9: 80px;
}
```

### Container / 形狀

```css
:root {
  --container-max: 1100px;
  --container-wide: 1800px; /* compare 區 G */

  /* 響應式 radius（§6-B 覆蓋） */
  --radius-pill: 50px; /* 膠囊按鈕（不隨斷點改變） */
  --radius-block: 24px; /* 深紫大區塊；1440+ → 80px */
  --radius-brand: 8px 8px 24px 8px; /* 招牌非對稱；1440+ → 8px 8px 40px 8px */
  --radius-card: 24px;
  --radius-tag: 50px; /* 小標籤膠囊 */
}
```

---

## 8. Assets 對應表

> 以 fig VFS 路徑（`fig_copy_files` 取出）為準，**不要手刻 SVG**。

| 語意名稱              | 用於 | fig VFS 路徑（`/web/`）                       | 格式 | 建議放置             |
| --------------------- | ---- | --------------------------------------------- | ---- | -------------------- |
| `hero-bg`             | B    | `section-one-bg.jpg`                          | JPG  | `src/assets/images/` |
| `logo-lockup`         | B    | `educate-logo-title.svg`                      | SVG  | `src/assets/images/` |
| 聯合報 logo           | A    | header 內向量（無獨立檔）                     | SVG  | 需設計師提供 ⚠️      |
| `icon-no-ads`         | B    | `no-ads.svg` / `no-ads-s.svg`                 | SVG  | `src/assets/icons/`  |
| `icon-exclusive`      | B    | `exclusive.svg` / `exclusive-s.svg`           | SVG  | `src/assets/icons/`  |
| `icon-report`         | B    | `report-s.svg`                                | SVG  | `src/assets/icons/`  |
| `edu-icon-promotion`  | C    | `edu-icon-promotion.svg`                      | SVG  | `src/assets/icons/`  |
| `edu-icon-trend`      | C    | `edu-icon-trend.svg`                          | SVG  | `src/assets/icons/`  |
| `edu-icon-admissions` | C    | `edu-icon-admissions.svg`                     | SVG  | `src/assets/icons/`  |
| `avatar-professor`    | D    | `avatar-professor.svg`                        | SVG  | `src/assets/images/` |
| `avatar-teacher`      | D    | `avatar-teacher.svg`                          | SVG  | `src/assets/images/` |
| `avatar-phd`          | D    | `avatar-PhD.svg`                              | SVG  | `src/assets/images/` |
| `avatar-parents`      | D    | `avatar-parents.svg`                          | SVG  | `src/assets/images/` |
| `author-feng`         | E    | `author-feng.jpg`                             | JPG  | `src/assets/images/` |
| `author-hsu`          | E    | `author-hsu.jpg`                              | JPG  | `src/assets/images/` |
| `discount-1`          | F    | `discount-1.png`                              | PNG  | `src/assets/images/` |
| `discount-2`          | F    | `discount-2.png`                              | PNG  | `src/assets/images/` |
| `compare-ad`          | G    | `compare-AD.svg`                              | SVG  | `src/assets/icons/`  |
| `compare-exclusive`   | G    | `compare-exclusive.svg`                       | SVG  | `src/assets/icons/`  |
| `compare-news`        | G    | `compare-news.svg`                            | SVG  | `src/assets/icons/`  |
| `compare-newspaper`   | G    | `compare-newspaper.svg`                       | SVG  | `src/assets/icons/`  |
| `compare-report`      | G    | `compare-report.svg`                          | SVG  | `src/assets/icons/`  |
| `compare-topics`      | G    | `compare-topics.svg`                          | SVG  | `src/assets/icons/`  |
| `award-1/2/3`         | H    | `award-1.jpg` / `award-2.jpg` / `award-3.jpg` | JPG  | `src/assets/images/` |
| `arrow-icon`          | 多處 | `arrow-icon.svg`                              | SVG  | `src/assets/icons/`  |
| `triangle`            | 多處 | `triangle.svg`                                | SVG  | `src/assets/icons/`  |
| `question-icon`       | D    | `question-icon.svg`                           | SVG  | `src/assets/icons/`  |
| `educator`            | 多處 | `educator.svg`                                | SVG  | `src/assets/icons/`  |

---

## 9. 圖片 vs HTML/CSS 判斷

| 元素                   | 建議做法                                    | 理由                                                   |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------ |
| 「高教視窗」標題       | **`<img>` SVG**                             | 含特殊商用字型（zihunbiantaoti），無法 web font        |
| 聯合報 logo            | **`<img>` SVG/PNG**                         | 品牌向量，不要 CSS 重做                                |
| Hero 背景              | **CSS `background-image`**                  | `center/cover`，有遮罩疊層                             |
| feature pill icons     | **`<img>` SVG**                             | 已有 SVG 素材，直接用                                  |
| 議題、比較、avatar SVG | **`<img>` SVG** 或 **inline SVG**（需換色） | 純展示用 img；需動態換色才 inline                      |
| 記者照、獎項圖         | **`<img>` JPG**                             | 已有照片素材                                           |
| 贈品圖                 | **`<img>` PNG**                             | 已有圖                                                 |
| 分隔線（Line3）        | **CSS** `border` 或 `<hr>`                  | 純 1px 線，CSS 即可                                    |
| 膠囊按鈕               | **HTML + CSS**                              | `border-radius: 50px`，CSS 完全重做                    |
| 品牌非對稱圓角容器     | **HTML + CSS**                              | `border-radius: 8px 8px 40px 8px`，無需圖              |
| 深紫大圓角區塊         | **HTML + CSS**                              | `border-radius: 80px`，背景色即可                      |
| 漸層背景（背景圖之間） | **CSS `linear-gradient`**                   | fig 有 `#F2EEFF → #472785 → #F2EEFF` 漸層，可 CSS 重做 |

---

## 10. 不確定 / 需詢問設計師（含假設）

### 必須確認

1. **聯合報 logo 原始檔**：fig header 內為散向量，無單一可用檔 → 請提供 logo SVG（含「數位版」字樣處理方式）。
2. **`744-s22`、`1024-s22` 詳細結構**：本規格 §6 表中「—」均為推估。尤其：
   - C section 背景色在哪個斷點從紫轉白？（推測 1024px，待確認）
   - feature pills 在 744px 是 row 還是 column？
   - border-radius 24px → 80px 的切換點在 744 還是 1024？
3. **CTA 連結目的地**：所有「訂閱…」按鈕導向哪個 URL？
4. **「限時優惠」日期（5/25）**：是靜態文案，還是需動態帶入？
5. **Header 定位方式**：fig 是 `position: absolute` 疊在 hero 上。實際需求是 `sticky`（滾動跟隨）還是只在 hero 上 absolute？

### 已採用的假設（如有誤請指正）

- A1：顏色 hex 由 fig METADATA `rgb()` 直接換算，精確度高。
- A2：主標字體用 Noto Serif TC Bold 替代 FOT-Chiaro Std B；「高教視窗」維持 SVG 圖。
- A3：C section 背景色的桌/行動差異（白 vs 紫）已由 fig 確認；切換斷點假設為 `min-width: 1024px`。
- A4：border-radius 響應式（24px → 80px）切換點假設為 `min-width: 1440px`；待 1024-s22 讀取後確認。
- A5：G section（比較卡）行動版為橫向滑動 + scroll-snap，無 fig 行動版確認，純推測。
- A6：footer 連結項目清單實作時再依 `footer-information` 元件補充。
- A7：spacing 統一規整為 8px scale（fig 有非整數間距，已歸一）。

---

## 11. 實作順序建議

```
① tokens.css（§7）
    ↓
② site-header（Section A）
    ↓
③ hero（Section B）
    ↓
④ issues（Section C）← 注意行動/桌面背景色差異
    ↓
⑤ need（Section D）
    ↓
⑥ authors（Section E）
    ↓
⑦ discount（Section F）
    ↓
⑧ compare（Section G）← 注意橫向滑動
    ↓
⑨ awards（Section H）
    ↓
⑩ footer（Section I / J）
```

每段先切 **desktop（1440px）**，再補 **mobile（320px）**，最後補中間斷點（744 / 1024）。
