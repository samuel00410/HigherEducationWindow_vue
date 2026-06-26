# 高教視窗 — 首頁開發規格（Homepage Dev Spec）

> 給 Claude Code 後續分段實作用。**本文件只涵蓋首頁**。
> 範圍依 `page-goal.md.txt`：Header / Hero / 主要內容卡片區 / 共用區 / Footer。
> 不實作：登入、後台、搜尋互動、複雜動畫、表單送出。
>
> 資料來源優先序：① screenshots ② notes ③ `.fig`（補充確認）。
> tokens / 區塊尺寸由 `.fig` 的 `1920` frame（node `81:70024`）讀出，其餘斷點以 mobile screenshot + 推論為準（見 §9）。

---

## ⚠️ 先讀：技術前提衝突（需設計師 / PM 確認）

| 來源                     | 技術指示                                                                    |
| ------------------------ | --------------------------------------------------------------------------- |
| 本次聊天指示（採用）     | 純 HTML + CSS，**不使用** Tailwind / Bootstrap / React，Flexbox / Grid 優先 |
| `page-goal.md.txt` notes | Vue3 + Vite + **Tailwind**                                                  |

**本規格以「純 HTML + CSS」撰寫**（聊天指示為準、且較新）。
若實際要走 Vue3 + Tailwind，§4 的語意結構仍適用，§5/§7 的 token 改寫成 `tailwind.config` 即可。**動手前請先確認走哪條路線。**

---

## 1. 首頁 Frame 對應（Breakpoints）

| 角色         | 設計寬度        | Figma frame                             | 對應 screenshot             |
| ------------ | --------------- | --------------------------------------- | --------------------------- |
| mobile       | 320px（最窄版） | `web/320`（另有 375）                   | `home-mobile.png` (320×568) |
| tablet       | 1024px          | `web/1024`                              | `home-1024.png` (1024×1366) |
| desktop      | 1440px          | `web/1440`                              | `home-1440.png` (1440×900)  |
| wide desktop | 1920px          | `web/1920`（node 81:70024，內容最完整） | `home-1920.png` (1920×1080) |

> 另有 `744`（直式平板）、`1280` 等中間 frame，本期可暫不單獨對應，用區間 RWD 自動撐開即可（見 §6）。
> 建議斷點：`--bp-md: 744px`、`--bp-lg: 1024px`、`--bp-xl: 1440px`。

---

## 2. 主要 Section 拆解（由上而下）

> screenshots 只拍到 §2-A～§2-C；§2-D 以下由 `.fig` 1920 frame 確認。

| #   | Section                      | 內容重點                                                                                                                                                                                                                                           | 背景                        |
| --- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| A   | **Header bar**               | 左：聯合報 logo +「數位版」；右：膠囊外框按鈕「訂閱享首年85折」                                                                                                                                                                                    | 品牌紫 `#472785`，高 60px   |
| B   | **Hero / section01**         | 全幅畢業生背景圖；置中「高教視窗」標題鎖定圖（含上方小標「全台最有深度的高教新聞」）；副標「升等、教育變革…助你領先業界！」；下方膠囊外框，內含 **3 個 feature pill**：①全站無跳出廣告 ②3000+篇高教獨家新聞 ③1萬+教育專業人士選擇                  | 背景照片，高約 1080（wide） |
| C   | **3大高教議題**              | 主標「最受關注的3大高教議題」+副標；淺紫卡片容器內 **3 欄 issue 卡**（學術升等指南 / 學界勞資與退休趨勢 / 招生與學術趨勢），每卡：icon＋標題＋2 條 bullet，欄間有分隔線；下方桃色 CTA 條：「訂閱後，你將能搶先閱讀…」+ 標籤 + 「訂閱首年85折」按鈕 | 白 + 淺紫卡 `#F2EEFF`       |
| D   | **你其實比想像中更需要訂閱** | 主標+副標「這些疑問將在《聯合報數位版》找到解答」；**4 個 answer 卡**（avatar＋角色＋提問）                                                                                                                                                        | 深紫圓角區塊 `#3C1375`      |
| E   | **口碑陣容**                 | 主標「口碑陣容」副標「深耕高教領域記者」；**2 張記者卡**（照片＋姓名＋簡介）                                                                                                                                                                       | 白                          |
| F   | **限時優惠**                 | 主標「【限時】…前享雙重優惠」；**2 個 discount 卡**（贈品圖＋說明）；CTA 按鈕＋訂閱服務電話資訊                                                                                                                                                    | 深紫圓角區塊 `#3C1375`      |
| G   | **他網 vs 聯合報數位版**     | 主標+副標「6大優勢…」；**6 張比較卡**（橫向排列 / 行動裝置可滑動）                                                                                                                                                                                 | 深紫區塊 `#472785`，上圓角  |
| H   | **國內外大獎肯定**           | 主標+副標；**3 張獎項卡**（圖＋說明）                                                                                                                                                                                                              | 白                          |
| I   | **Footer-information**       | 連結 / 聯絡資訊區（共用區）                                                                                                                                                                                                                        | —                           |
| J   | **Educate Footer**           | 版權列                                                                                                                                                                                                                                             | —                           |

---

## 3. 可重用元件（Reusable Components）

對應 `.fig` component families（節點名稱供 Claude Code 對照查證）：

| 元件                           | 用於        | 變體 / props                        | fig 來源                                  |
| ------------------------------ | ----------- | ----------------------------------- | ----------------------------------------- |
| `site-header`                  | A           | logo、CTA 文案                      | `educate-header`（2 變體）                |
| `cta-button`（膠囊主按鈕）     | C/F/H…      | label、size、實心/外框兩型          | `educate-button`（2 變體）                |
| `feature-pill`                 | B           | icon、雙行文字                      | `feature`（3 變體）                       |
| `section-heading`（主標+副標） | C/D/E/F/G/H | title、subtitle、字色（深底用淺色） | `educate-main-title`（3 變體）            |
| `issue-card`                   | C           | icon、title、bullets[]              | `edu-issues`（3 變體）                    |
| `answer-card`                  | D           | avatar、role、question              | `avatar`（5）+ `answer-ta`                |
| `author-card`                  | E           | photo、name、bio                    | `author`（3 變體）                        |
| `discount-card`                | F           | image、title、desc                  | `discount-box`                            |
| `compare-card`                 | G           | icon、標題、他網 vs 本站對照        | `Comparison card`（2）/ `newspapaer`(6)   |
| `award-card`                   | H           | image、caption                      | `Frame 23`（2 變體）                      |
| `divider`（細直線/橫線）       | C 等        | 方向                                | `Line 3`                                  |
| `footer-info` / `footer-bar`   | I / J       | —                                   | `footer-information`(3)、`educate-footer` |

> 圖示（icon）建議全部當「素材」處理（§8），不要手刻 SVG。

---

## 4. HTML 語意結構建議

純 HTML，BEM-ish class 命名（`block__element--modifier`）。骨架：

```text
<header class="site-header"> … logo + cta </header>

<main>
  <section class="hero">                 ← B
    <div class="hero__inner">
      <img class="hero__title-lockup">   ← 高教視窗 SVG
      <p class="hero__subtitle">
      <ul class="feature-pills">
        <li class="feature-pill"> … ×3
      </ul>
    </div>
  </section>

  <section class="issues">               ← C
    <header class="section-heading"> h2 + p </header>
    <div class="issues__grid">           ← 3 欄
      <article class="issue-card"> … ×3
    </div>
    <aside class="issues__cta"> … </aside>
  </section>

  <section class="need">                 ← D
    <header class="section-heading">
    <ul class="answer-grid"><li class="answer-card"> … ×4
  </section>

  <section class="authors"> … ×2 </section>          ← E
  <section class="discount"> … ×2 + cta </section>   ← F
  <section class="compare">                          ← G
    <div class="compare__track"><article class="compare-card"> … ×6
  </section>
  <section class="awards"> … ×3 </section>           ← H
</main>

<footer class="site-footer">
  <div class="footer-info"> … </div>     ← I
  <div class="footer-bar"> … </div>      ← J
</footer>
```

語意要點：

- 每個 section 第一個元素用 `<h2>`（頁面唯一 `<h1>` 給 Hero 標題，可用 `aria-label="高教視窗"` 掛在 lockup 圖上）。
- 卡片用 `<article>`；圖示為純裝飾 → `alt=""` / `aria-hidden`。
- bullet 用 `<ul><li>`；feature pill 一組也用 `<ul>`。
- CTA 是連結就用 `<a class="cta-button">`，非送出表單。

---

## 5. CSS Layout 策略

- **不使用框架**；以 CSS 自訂屬性（tokens，§7）＋ Flexbox / Grid。
- **避免固定高度**（notes 要求）：用 `padding` 撐高、`min-height` 例外才用；卡片等高交給 grid/flex 拉伸。
- **置中容器**：
  ```css
  .container {
    width: 100%;
    max-width: var(--container-max);
    margin-inline: auto;
    padding-inline: var(--gutter);
    box-sizing: border-box;
  }
  ```
  `--container-max: 1100px`（內容區）；比較區 G 可放寬到 `1800px`。
- **區塊內距**：`section { padding-block: var(--section-pad-y); }`（desktop 80px、mobile 縮小，§6）。
- **多欄卡片**用 Grid：
  - issues（C）：`grid-template-columns: repeat(3, 1fr)`；分隔線用 `gap` + 偽元素或 `<hr>`。
  - need（D）：`repeat(4, 1fr)`。
  - awards（H）：`repeat(3, 1fr)`。
  - authors（E）/ discount（F）：`repeat(2, 1fr)`。
- **比較區（G）**橫向 6 卡：desktop 用 flex 平均分配；窄螢幕改 `overflow-x:auto` 橫向滑動（`scroll-snap` 可選，屬輕互動，非「複雜動畫」）。
- **品牌圓角形狀**：深紫大區塊 `border-radius: 80px`（G 為上圓角 `80px 80px 0 0`）；卡片容器有一個招牌「不對稱圓角」`border-radius: 8px 8px 40px 8px`，建議做成 token / utility class 複用。
- **Hero 背景**：`background: url(...) center/cover no-repeat;` 文字加深色遮罩或陰影確保對比。

---

## 6. RWD 對應方式

採 **mobile-first**，min-width media query 往上加。各斷點主要變化：

| 區塊               | mobile (320)                                   | tablet (744–1024) | desktop (1440+) |
| ------------------ | ---------------------------------------------- | ----------------- | --------------- |
| Header CTA         | 維持顯示（可縮小 padding）                     | 同                | 同              |
| Hero feature pills | **直向堆疊**（screenshot 確認 pill 換行/堆疊） | 2 欄或單列        | 單列 3 個並排   |
| issues（C）        | 1 欄堆疊                                       | 1–2 欄            | 3 欄            |
| answer（D）        | 1 欄                                           | 2 欄              | 4 欄            |
| authors（E）       | 1 欄                                           | 2 欄              | 2 欄            |
| discount（F）      | 1 欄                                           | 2 欄              | 2 欄            |
| compare（G）       | 橫向滑動                                       | 橫向滑動 / 換行   | 6 卡並排        |
| awards（H）        | 1 欄                                           | 2–3 欄            | 3 欄            |
| 區塊內距           | 約 40px                                        | 約 60px           | 80px            |
| 主標字級           | 約 28–30px                                     | 約 36px           | 44px（見 §7）   |

建議：

```css
:root {
  --section-pad-y: 40px;
}
@media (min-width: 744px) {
  :root {
    --section-pad-y: 60px;
  }
}
@media (min-width: 1024px) {
  :root {
    --section-pad-y: 80px;
  }
}
```

卡片網格用 `grid-template-columns: repeat(auto-fit, minmax(min(100%, 260px), 1fr))` 可大幅減少斷點手寫量（固定欄數需求高的區塊再用明確 media query 覆蓋）。

---

## 7. Design Tokens

> 由 `.fig` METADATA + 1920 frame 讀出。命名建議；hex 為四捨五入後的近似（請以 §9 提到的最終確認為準）。

### 顏色

```css
:root {
  /* 品牌紫 */
  --c-brand: #472785; /* rgb(71,39,133)  header / 區塊G底 / 主標字 */
  --c-brand-deep: #3c1375; /* rgb(60,19,117)  深紫區塊 D/F */
  --c-brand-tint: #f2eeff; /* rgb(242,238,255) 淺紫卡底 */
  --c-violet-soft: #6452ac; /* rgb(100,82,172) 次要紫 */

  /* 桃 / 米 */
  --c-peach: #ffe7d6; /* rgb(255,231,214) CTA 條底 / 副標字(深底上) */
  --c-cream: #fff5ab; /* rgb(255,245,171) 強調黃(優惠標) */

  /* 紅（聯合報 logo / 「獨」標） */
  --c-accent-red: #cb2529; /* rgb(203,37,41) */

  /* 中性 */
  --c-text: #232323; /* rgb(35,35,35) 主文 */
  --c-text-muted: #67645c; /* rgb(103,100,92) */
  --c-white: #ffffff;
  --c-line: #d9d9d9; /* rgb(217,217,217) 分隔線 */
}
```

### 字體

```css
:root {
  /* 內文：黑體 */
  --font-sans: 'Noto Sans TC', system-ui, 'Helvetica Neue', Arial, sans-serif;
  /* 標題：襯線（主標「最受關注的…」為紫色襯線 Bold） */
  --font-serif: 'Noto Serif TC', serif;
}
```

- 大標「高教視窗」是**特殊顯示字體**（fig 用 zihunbiantaoti / 思源黑 Heavy 等），已輸出為 SVG lockup → **當圖片用，不要用 web font 重打**（§8）。
- 字重：內文 Regular 400、強調 Bold 700。

### 字級（desktop 基準，mobile 依 §6 縮放）

```css
--fs-hero-sub: 30px; /* hero 副標 / CTA 標題 */
--fs-h2: 44px; /* 區塊主標 (Noto Serif TC Bold) */
--fs-h2-sub: 24px; /* 區塊副標 */
--fs-card-title: 24–30px;
--fs-body: 18–20px;
--fs-small: 16px;
```

### Spacing（建議 8px scale）

```css
--space-1: 8px;
--space-2: 12px;
--space-3: 16px;
--space-4: 24px;
--space-5: 32px;
--space-6: 40px;
--space-7: 48px;
--space-8: 56px;
--space-9: 80px;
```

觀察到的實際值：區塊內距 80px、欄間距 24/40/56px、卡片內 gap 12/24px、header padding `12px 15px`。

### Container / 形狀

```css
--container-max: 1100px; /* 內容 */
--container-wide: 1800px; /* 比較區 G */
--gutter: 48px; /* desktop；tablet 24、mobile 16 */
--radius-pill: 50px; /* 膠囊按鈕/外框 */
--radius-block: 80px; /* 深紫大區塊 */
--radius-brand: 8px 8px 40px 8px; /* 招牌不對稱圓角 */
--radius-card: 24px;
```

---

## 8. Assets 對應表

> 全部從 `.fig` 匯出，**不要手刻 SVG**。路徑為 fig VFS（`fig_copy_files` 取出）。

| 用途                   | 區塊 | 檔案（fig VFS `/web/...`）                                                                                                              | 格式    |
| ---------------------- | ---- | --------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| 「高教視窗」標題鎖定圖 | B    | `educate-logo-title.svg`                                                                                                                | SVG     |
| 聯合報 logo（header）  | A    | 待匯出（fig header 內為向量，無單檔）→ 需設計師給 logo 原始檔                                                                           | SVG/PNG |
| Hero 背景照（畢業生）  | B    | `section-one-bg.jpg`（image `30561cb494f77b28.jpg`）                                                                                    | JPG     |
| feature pill icons     | B    | `no-ads.svg`(無廣告) / `exclusive.svg`(獨家) / `report-s.svg`(專業人士)                                                                 | SVG     |
| 3議題 icons            | C    | `edu-icon-promotion.svg`(升等) / `edu-icon-trend.svg`(勞資退休) / `edu-icon-admissions.svg`(招生)                                       | SVG     |
| answer avatars         | D    | `avatar-professor.svg` / `avatar-teacher.svg` / `avatar-PhD.svg` / `avatar-parents.svg`                                                 | SVG     |
| 記者照                 | E    | `author-feng.jpg` / `author-hsu.jpg`                                                                                                    | JPG     |
| 優惠贈品圖             | F    | `discount-1.png` / `discount-2.png`                                                                                                     | PNG     |
| 比較卡 icons           | G    | `compare-AD.svg` / `compare-exclusive.svg` / `compare-news.svg` / `compare-newspaper.svg` / `compare-report.svg` / `compare-topics.svg` | SVG     |
| 獎項圖                 | H    | `award-1.jpg` / `award-2.jpg` / `award-3.jpg`                                                                                           | JPG     |
| 其他                   | 多處 | `arrow-icon.svg`、`triangle.svg`、`question-icon.svg`、`educator.svg`                                                                   | SVG     |

> 命名建議統一改為語意化（如 `icon-no-ads.svg`、`hero-bg.jpg`）後放 `src/assets/`。

---

## 9. 不確定 / 需詢問設計師（含假設）

**必須確認：**

1. **技術路線**：純 HTML+CSS vs Vue3+Tailwind（兩來源衝突，見頂部）。本規格暫採純 HTML+CSS。
2. **聯合報 logo 原始檔**：fig header 內為散向量，無單一可用檔 → 請提供 logo SVG/PNG（含「數位版」字樣處理方式）。
3. **斷點細節版型**：fig 的 `1024` / `1440` / `320` frame 在重建資料中為空，**僅 `1920` frame 內容完整**。tablet/desktop/mobile 的逐區欄數與字級目前是**從 1920 + mobile screenshot 推論**，請設計師確認各斷點切版（尤其 §6 表格）。
4. **比較區（G）行動裝置行為**：橫向滑動還是堆疊換行？（假設：窄螢幕橫向滑動 + snap）
5. **CTA 連結目的地**：所有「訂閱…」按鈕導向哪個 URL？（本期不做表單，僅 `<a>`）

**已採用的假設（如有誤請指正）：**

- A1 色票 hex 為 RGB 近似值，正式上線前以設計師色票為準。
- A2 主標字體用 Noto Serif TC 替代 fig 內的商用字（FOT / 思源 Heavy 等）；「高教視窗」維持 SVG 圖。
- A3 §2 §D 以下（need/authors/discount/compare/awards/footer）screenshots 未涵蓋，結構依 fig 1920 frame；文案/圖以 fig 為準。
- A4 spacing 統一規整為 8px scale（fig 有非整數值如 56/40/24，已歸一）。
- A5 「限時優惠」日期（5/25）為 fig 既有文案，是否動態請確認。
- A6 footer 連結項目清單未逐項列出，實作時再依 `footer-information` 元件補。

---

### 交付範圍備註

本文件為**規格**，依限制**未含完整 HTML/CSS**。建議 Claude Code 後續分段順序：
`tokens(§7) → site-header(A) → hero(B) → issues(C) → 其餘卡片區(D–H) → footer(I/J)`，每段先切 desktop 再補 RWD。
