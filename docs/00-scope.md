# PIXEL MART scope

## 鎖定
- 教學流程 = `value/` 原版（對貨 + 櫃檯付足）
- 操作 = 大掣／點圖，唔使十字走位
- 比錢 = 符號三選，唔數銀
- 三個 mode 共用同一條流程骨：**門口 → 貨架 → 核對 → 對貨 → 櫃檯三選 → 結果 → 證狀**

## 活動週 2026-11-23～27 路由
| 對象 | mode | 注意 |
|---|---|---|
| 中學形狀（週主推） | `shape` | 封面推介；中卡先；AUTO_WALK on |
| 中學數量 | `secondary` | 即用，唔等 value 教材二 |
| 小學相同圖 | 優先 plain `sen-unmanned-store` | 本盤 primary 只係試皮 |

星只獎比足；形錯≠不誠實；禁分類籃冒充誠信。

## 禁止
- open-world 超市
- 多層選單／背包
- 即時 NPC 劇透對錯
- 覆蓋 `sen-unmanned-store` Live
- 自由地圖／數銀／多房（永遠唔做）

## 同步
原版 logic 修好 → diff 進本盤 JS；CSS 皮保留本盤。

---

## 三個 mode（v1.6 定案）

| Mode | `MODES` key | 清單格式 | 跟單定義 | 星星 |
|---|---|---|---|---|
| 小學 | `primary` | `items: [id, …]` | 呢幾樣嘢一樣都冇漏冇多 | ⭐ = 付足 |
| 中學·數 | `secondary` | `items: [{id,qty}, …]` | 每樣數量啱 | ⭐ = 付足 |
| 中學·形 | `shape` | `shapes: [{shape,qty}, …]` | 每種形狀嘅**數量**啱 | 🥈 = 跟單 · ⭐ = 跟單 + 付足 |

`MODES` 嘅 `qty` / `shape` flag 係唯一 mode 開關；邏輯全部靠 `isQtyMode()` / `isShapeMode()` 分流。

### 中學·形 設計定案
（原文註解）
```
數1–5；●▲■入單；▬干擾；供≥需+餘
銀=跟單；金=跟單+比足。形錯≠不誠實。
```

逐句拆：**●▲■ 入單** = 圓/三角/正方形可以出現喺清單 · **▬ 干擾** = 長方形只做干擾 ·
**供 ≥ 需 + 餘** = 貨架供應要夠用仲要有多餘 · **銀 = 跟單 / 金 = 跟單 + 比足** ·
**形錯 ≠ 不誠實**。

⚠️ **「數1–5」定案**：每種形狀 qty ∈ 1–5，**且**每單總件數建議 ≤5（中度負荷）。  
現行卡：中總5、高總4；每形 1–2 —— 未頂滿上限，夠活動週用。

**供應表**（貨架 9 個位，`MAP_LAYOUT` row 2/4/6 × x=2,4,6）

| 級 | 清單需求 | 貨架供應 | 餘量 |
|---|---|---|---|
| 中 | ●2 ▲1 ■2（合共 5 件） | ●3 ▲2 ■3 ▬1（9 件） | 每種都有餘；1 件長方形干擾 |
| 高 | ●2 ▲1 ■1（合共 4 件） | ●3 ▲2 ■1 ▬2（8 件） | ■ 剛好 1 件（無餘）；2 件長方形干擾；1 個空架位 |

⚠️ 現有資料**每種形狀只用到 1–2 件**，遠未撳到「數1–5」嘅上限。
日後要放大數量，先確認貨架供應夠（`LISTS_SHAPE[].shelf` 加貨要同時加 `assets/shape/` 圖）。

**形狀 ↔ 貨**
| 形狀 | badge | 代表圖 | 貨 |
|---|---|---|---|
| 圓形 | ● | `c1` 餅乾盒 | c1 餅乾盒 · c2 曲奇罐 · c3 布甸杯 |
| 三角形 | ▲ | `t1` 蛋糕盒 | t1 蛋糕盒 · t2 飯糰 |
| 正方形 | ■ | `s1` 紙巾盒 | s1 紙巾盒 · s2 朱古力盒 · s3 豆腐盒 |
| 長方形 | ▬ | `r1` 果汁盒 | r1 果汁盒 · r2 牛奶盒（**干擾**，唔入清單） |

清單用 `SHAPE_REP`（每形狀代表一件），唔綁死成單一貨 → 學生見到三款圓形，
要自己認形狀 + 自己數件數。

### 誠信語意（重要）
| 情況 | 判斷 | 星星 | 會唔會記「對貨有落差」 |
|---|---|---|---|
| 跟單 + 比足 | 金 | ⭐ | 唔會 |
| 跟單 + 冇比足 | 銀 | 🥈 | 唔會 |
| 形狀／數量錯 + 比足 | 只講「你有付足」 | — | **唔會**（形錯 ≠ 不誠實） |
| 形狀／數量錯 + 冇比足 | — | — | 唔會 |
| 對貨答「一樣」但實際唔一樣 | 有落差 | — | **會**（唯一嘅誠信問題） |

誠信只由「對貨答覆 vs 實際」決定，**唔由攞錯嘢決定**。

---

## 皮／骨分界（v1.5 重繪之後）

v1.5 換咗成個地圖嘅 **render 層**（canvas pixel-art），但**教學邏輯一行冇改**：
`hotspotNear()` / `pickItemFromMap()` / `doInteract()` / `goToCheckout()` 全部原封不動。

| 屬邊 | 內容 |
|---|---|
| 骨（唔准喺本盤亂改） | 清單 → 核對 → 對貨 → 三選付費 → 星星條件 |
| 皮（隨時可換） | `PAL` 色板 · `drawKid` · `drawFloor` · `drawShelf` · `drawCounter` · `drawDoor` · `drawBang` |

v1.6 加咗中學·數／中學·形兩個 mode，**係骨嘅延伸**（清單格式 + 判分 + 星星），
唔係新流程。

兩個開關：
- `AUTO_WALK`（`true`）— 㩒遠格自動行過去。**只行唔互動**：攞唔攞一定要學生自己撳，
  自動代佢決定會直接破壞誠信學習目標。
- `MAP_LAYOUT` — 11×9 字串地圖。`#` 牆 · `.` 地 · `S` 架 · `C` 櫃檯 · `D` 門。

---

## 陷阱（加新 mode 前必讀）

### `listIds()` 喺唔同 mode 回傳唔同嘢
```js
function listIds(list){
  if(list.shapes) return list.shapes.map(s => s.shape);   // ← 形狀名，唔係 item id！
  return list.items.map(i => typeof i === "string" ? i : i.id);
}
```
形狀 mode 回傳 `"circle"` / `"triangle"` / `"square"`。任何假設 `listIds()` 一定係 item id
嘅地方，喺形狀 mode 會攞到 `ITEMS["circle"]` → `undefined` → 讀 `.name` 就死機。

**呢個就係 v1.7 修嘅 crash**（形狀 mode 一入地圖 100% 死）：
```js
// renderMap() 錯版
const ids = listIds(L).concat(shelfIds(L));
// renderMap() 修版
const ids = isShapeMode() ? shelfIds(L) : listIds(L).concat(shelfIds(L));
```
（形狀 mode 全部實物都喺貨架，清單只描述形狀 → 淨係擺 `shelfIds()`。）

**守則**：加新 mode 嗰陣，`listIds()` 嘅**每個 call site** 都要重新睇一遍
（而家只有 `renderMap()` 一處會喺形狀 mode 出事；`renderDoor` / `renderListStrip` /
`renderCheck` / `answerCheck` / `judge` 全部喺 shape 分支跳過）。

### 未知 item id 唔好炸成個畫面
`ITEMS[id]` 可能係 `undefined`。直接寫 `ITEMS[id].name` 會令成個地圖 render 死機，
之後所有嘢都撳唔到。v1.7 改成 `const it = ITEMS[id]; it ? … : "貨架"`。

### 切 mode 要清晒 state
`setMode()` 要清 `basket` / `basketN` / `checks` / `cursor` / `declared` / `declareAns` /
`paid` / `payChoice` / `silvers` / **`stars`** / **`log`**。
（v1.7 補咗 `stars` + `log` —— 之前只清 `silvers`。）

### 唔寫 localStorage / sessionStorage
防止上一個學生嘅資料污染下一個學生。老師記錄面板只存記憶體，老師手動複製走。

---

## 未驗證路徑
- **方向掣 ▲▼◀▶ / 鍵盤 ↑↓←→ 逐步行** — Browser 自動化送唔到具名按鍵，未經自動化驗證。
  「㩒格仔 BFS 行過去」＋「撳掣互動」已實測通過。
