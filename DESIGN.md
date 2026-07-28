# 陌花 MOHUA — Design.md

一份給 AI 編碼助手（Claude Code 等）與人類開發者共用的設計系統文件。
在對 `index.html` / `course.html` / `style.css` 做任何視覺調整前，先讀這份文件，維持風格一致。

參考資源（人工參考用，非自動抓取）：
- 元件靈感／參考：https://21st.dev/（React/Tailwind 元件市集，本專案為純 HTML/CSS，需手動轉譯成 vanilla 樣式才能使用）
- 配色／漸層靈感：https://kinetics.colorion.co/
- 更多 design.md 範例庫：https://github.com/VoltAgent/awesome-design-md ／ https://getdesign.md/

---

## 1. Visual Theme & Atmosphere

陌花 MOHUA 是一間手作甜點與烘焙課程工作室的網站。設計基調是：

- **老派、溫暖、克制**——像老公寓五樓的小工作室，不喧嘩、不促銷感。
- **文青質感**：襯線字體、義大利體標語、米杏色調，帶一點日式與台灣在地感。
- **留白與呼吸感**：內容欄寬固定在 900px，段落行距寬鬆（line-height 1.85）。
- 不用高飽和度色彩、不用強烈陰影或漸層特效；質感靠色調、字體與間距堆疊出來，而不是裝飾元件。

## 2. Color Palette & Roles

以 CSS 變數定義於 `:root`，並支援亮／暗模式（`prefers-color-scheme` + `data-theme` 手動切換）。

| 變數 | 亮色 | 暗色 | 用途 |
|---|---|---|---|
| `--bg` | `#fbf9f5` | `#1a1715` | 頁面背景，米白／深棕黑 |
| `--ink` | `#2b2724` | `#ece6de` | 主要文字色 |
| `--ink-soft` | `#8c8175` | `#a89a89` | 次要／說明文字色 |
| `--accent` | `#a3814f` | `#c9a876` | 主強調色（焦糖金），CTA、價格、連結、tab active |
| `--accent-2` | `#7c8570` | `#9aab86` | 次強調色（灰橄欖綠），目前使用較少，留給第二層強調 |
| `--line` | `rgba(43,39,36,.10)` | `rgba(236,230,222,.14)` | 邊框、分隔線 |
| `--card` | `#f5f2ea` | `#241f1c` | 卡片背景 |
| `--card-hover` | `#ece6d9` | `#2d2723` | 卡片 hover／次要底色 |
| `--btn-ink` | `#fff` | `#1a1715` | 按鈕上文字色（依主題反轉） |

**用色原則：**
- 強調色（`--accent`）只用在需要被注意的地方：CTA 按鈕、價格、作用中的 tab、連結 hover、pull-quote。避免大面積使用。
- 不新增第三種強調色；需要區分層級時，靠 `--ink` / `--ink-soft` 的對比即可。
- 暗色模式的色值已經調整過對比與飽和度（不是單純反白/反黑),新增元件時要同時檢查兩種模式。

## 3. Typography

- **內文／標題字體**：`"凝書體","臺灣道路體","LXGW WenKai TC","霞鶩文楷",Georgia,"Noto Serif TC",serif`——中文襯線體優先，帶手寫感與東方氣質。
- **UI 元件字體**（按鈕、導覽、標籤）：`.ui` class → `-apple-system,"PingFang TC","Microsoft JhengHei",sans-serif`。凡是「介面操作元件」（nav、tab、按鈕、價格標籤）都應加上 `.ui`，內文敘述維持預設襯線體。
- **強調斜體**：`Georgia,serif` + `font-style:italic`，用在 `.tagline`、`.pull-quote`、hero 標題等「輕文青語氣」的短句。
- **字級**：用 `clamp()` 做響應式字級（如 `h1.name{font-size:clamp(2rem,6vw,2.6rem)}`），不要寫死單一 px/rem。
- **行距**：body 預設 `line-height:1.85`，維持寬鬆閱讀感，不要因為新元件而改小。

## 4. Component Styling

- **按鈕**：一律圓角膠囊（`border-radius:999px`），實心用 `--accent` 底 + 白字；次要動作用 `outline`（透明底 + `--accent` 邊框與文字）；停用狀態降低透明度並移除 pointer-events，不要用刪除線或紅色。
- **卡片**（`.product-card`, `.info-card`, `.course-card`）：`--card` 底色、`14–16px` 圓角、`1px solid var(--line)` 邊框，不用陰影（`box-shadow`）做立體感，靠底色差與邊框即可。
- **輸入框／表單**：`8px` 圓角、`1px solid var(--line)` 邊框、`--card` 或 `--card-hover` 底色，focus 狀態沿用瀏覽器預設，不額外做花俏外框。
- **Tag／Tab**：膠囊形狀，active 狀態直接填滿 `--accent`。
- **分隔**：用 `1px solid var(--line)`（實線）或 `1px dashed var(--line)`（虛線，用在提示/備註類區塊，如 `.item-note`, `.warn-note`, `.text-list`）區分語意——虛線＝提醒/附註，實線＝結構分隔。

## 5. Layout Principles

- **內容寬度**：固定 `max-width:900px`，左右 `padding:0 20px`（`.wrap` / `.topnav-inner`）。不要做超寬螢幕的多欄大版面，這是一個窄欄、閱讀導向的網站。
- **區塊節奏**：`section.block{padding:68px 0}` + 底部 `1px solid var(--line)` 分隔，形成穩定的垂直節奏，新增區塊要沿用這個 padding。
- **Grid**：雙欄用 `grid-template-columns:1fr 1fr`，在 `max-width:600px` 斷點收成單欄（見 `.product-grid`, `.info-grid`）。
- **間距尺度**：常用 6 / 8 / 10 / 12 / 14 / 16 / 18 / 20 / 28 / 34 / 40 / 68px，避免自創新的隨意數值，優先從既有尺度挑選。

## 6. Depth & Elevation

- 全站沒有使用 `box-shadow` 做卡片陰影，立體感全靠底色分層（`--bg` → `--card` → `--card-hover`）與 `--line` 邊框。
- 唯一的「浮起」元件是購物車抽屜（`.drawer`）與其 `.overlay`，用位移 + 半透明黑色遮罩，不是陰影。
- 若要新增浮動元件（如 tooltip、dropdown），比照抽屜模式：`--card` 底 + `--line` 邊框 + 位移/透明度過場，不要引入陰影系統。

## 7. Design Guidelines（Do / Don't）

**Do**
- 新色彩一律加到 `:root` 的 CSS 變數，並同步補上暗色模式對應值。
- 互動元件（按鈕、tab、連結)使用 `.ui` 字體 class；敘述性文字維持襯線體。
- 圓角一致用膠囊（按鈕/標籤/tag）或 12–16px（卡片/圖片容器）。
- 響應式一律用 `clamp()` 或既有斷點（480px / 560px / 600px），不要新增任意斷點。

**Don't**
- 不要引入鮮豔色、漸層背景、強陰影或玻璃擬態以外的特效（僅 `.topnav` 用 `backdrop-filter:blur` 做輕微毛玻璃，其餘避免）。
- 不要用無襯線體寫品牌敘述文字（違反「老派文青」語氣）。
- 不要為單一元件新開一組間距/色彩數值，先檢查本文件第 2、5 節有沒有現成可用的。
- 不要假設有建置工具（React/Tailwind/npm）：這是純 HTML + CSS + 原生 JS 的靜態網站（見 `serve.js`），元件靈感（如 21st.dev）需手動轉譯成 vanilla 標籤與 class，不能直接安裝套件。

## 8. Responsive Behavior

- 斷點：`600px`（雙欄→單欄，多數 grid）、`560px`（gallery-strip 3→2 欄）、`480px`（nav 文字/按鈕縮小）。
- Hero 區塊在 `max-width:600px` 時提高高度（`78vh`）以保留視覺重量。
- 行動裝置優先確保：導覽列不換行（nav-links 用 `flex-shrink` 壓縮）、按鈕與可點擊區域維持可觸控大小（不小於約 32px 高）。

## 9. Agent Prompt Guide（給 AI 的速查）

修改此網站視覺時：
1. 先確認要動的是「敘述文字」還是「介面元件」，決定字體（襯線體 vs `.ui`）。
2. 新色彩/間距先查本文件第 2、5 節有沒有現成值可以重用。
3. 修改 `style.css` 後，同時檢查亮色與暗色模式（`prefers-color-scheme` 與 `[data-theme]` 兩處都要更新)。
4. 不加陰影、不加漸層特效、不改變 900px 內容欄寬，除非使用者明確要求。
5. 若要參考 21st.dev 元件或 kinetics.colorion.co 配色，先手動記錄下想要的色碼/佈局，再對照本文件語氣與色板手動改寫成 vanilla HTML/CSS，不要整段複製 React/Tailwind 代碼。
