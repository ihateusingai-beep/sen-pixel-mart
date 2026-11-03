# PIXEL MART · 8-bit 無人商店

**將軍澳培智學校**｜誠信價值觀學習週 × 數學科  
**形態** Game Boy / 超市 pixel **劇場皮**（非 open-world）

---

## 同原版關係

| | 路徑 | 角色 |
|---|---|---|
| **課堂主力** | `../value/` | plain UI · 已 Live · 穩定試堂 |
| **本 repo** | `value-8bit/` | **同一套流程骨** + 8-bit 皮 · 實驗／進階觀感 |

**骨（唔改教學紅線）**
- 門口 → 貨架（清單常駐）→ 核對 → 對貨（一樣／唔一樣）→ **櫃檯三選**（比足／比少／唔比）→ 結果 → 證狀
- 星 **只獎比足**
- 系統唔提前洩漏對錯
- 零依賴、離線、TTS、培智校徽

**皮（本盤先做）**
- **Pixel-art canvas 地圖**：內部 176×144（11×9 × 16px 格），CSS `pixelated` 放大
- 角色 4 向 + 3 幀走路（腿先畫、身體蓋髖位，同真 tile 角色分層）
- 錯位磚牆 / 棋盤地磚 + 固定雜訊 / 木架 / 收銀機 / 藍門
- 「！」提示浮喺熱點上面，黑邊黃底，兩幀上落
- 貨架上面嘅嘢**用 emoji 畫**（特教學生靠樣認嘢，唔好用抽象 pixel 取代）
- 文案 PRESS START / PIXEL MART
- **未做**：自由 roam、真銀找續、多房探索（刻意唔做）

**操作**
| 方式 | 行為 |
|---|---|
| 㩒地圖任意格 | BFS 自動行過去（**只行唔自動互動**） |
| 大掣 ▲▼◀▶ / Ａ | 逐步行；Ａ = 互動 |
| 鍵盤 ↑↓←→ / WASD / 空白 | 同大掣 |
| 「去櫃檯」 | 跳去結帳流程 |

---

## 三個學習模式（封面揀）

封面三張卡 → 揀 mode → 入地圖。**流程骨三個 mode 一模一樣**，分別只喺「要攞咩」同「咩叫跟單」。

| Mode | 標籤 | 學乜 | 清單格式 | 籃上限 |
|---|---|---|---|---|
| **小學** | 🚪 小學 | 對清楚 · 老實付足 | `items: ["apple","milk",…]`（每樣 1 件） | 6 |
| **中學·數** | 🔢 中學·數 | 數數 · 數字配對 | `items: [{id,qty},…]`（每樣要幾多件） | 6 |
| **中學·形** | ▲●■ 中學·形 | 跟形 · 唔好手多多 | `shapes: [{shape,qty},…]`（每種形狀要幾多件） | 9 |

### 中學·數（數字配對）
清單寫「蘋果 ×3、牛奶 ×2、餅乾 ×5」。㩒貨架 → 彈出數字掣面板（1–10）→ 揀要攞幾多件 → 入籃。  
貨架上嘅嘢**唔寫數字**（特教學生要自己數），數字只喺清單同掣面板出現。

### 中學·形（形狀購物）
清單寫「●×2 ▲×1 ■×2」—— **只講形狀同數量，唔講係咩貨**。  
貨架擺嘅係超市真貨圖（曲奇罐、蛋糕盒、紙巾盒、果汁盒…），每件貨右上角有形狀 badge（●▲■▬）。
學生要自己認形狀、自己數件數，睇到啱形先攞。

**銀／金雙星**（只有呢個 mode 有雙星）
| | 條件 | 意思 |
|---|---|---|
| 🥈 **銀** | 跟單（形狀 + 數量全部啱） | 忍住手，冇多攞 |
| ⭐ **金** | 跟單 **+** 櫃檯比足錢 | 誠信兩樣都做到 |

⚠️ **形錯 ≠ 不誠實**。形狀／數量錯淨係代表「下次要對清楚」，**唔會**判學生唔誠信。
付咗足錢但形狀唔啱 → 仍然講「你有付足」，只有「對貨答覆同實際唔一致」先係誠信落差（老師記錄會標）。

---

## 開機

```bash
open "index.html"   # 雙擊即開
```

| 檔 | 用途 |
|---|---|
| `index.html` | 單檔教材 |
| `assets/puichi-logo.png` | 校徽（離線） |
| `assets/shape/*.jpg` | 形狀 mode 超市真貨圖（10 張，無文字包裝） |
| `docs/` | 規格／演進筆記 |

---

## 路線圖（鎖死）

| 版 | 做 | 狀態 |
|---|---|---|
| **v0** | 複製原版骨 + 8-bit CSS 皮 | 完成 |
| **v1–v2** | 單屏地圖 · 角色格移 · 貨架／櫃檯熱點 · 去櫃檯捷徑 · 接 check/pay | 完成 `a348bf1` |
| **v1.5** | Pixel-art canvas 重繪（sprite / 動畫 / 浮「！」）+ 版面 bug 修復 | 完成 `1b80fc4` |
| **v2.5** | 模式 A 自動行路（㩒遠格 BFS 走过去） | 完成（`AUTO_WALK` 開關） |
| **v1.6** | 中學·數 mode · 中學·形 mode · 形狀真貨圖 | 完成 `9a89ba2` `b173b27` `06e883d` |
| **v1.7** | 形狀 mode crash 修復 · 防禦 guard · 文件更新 | **而家** |
| ❌ | 自由地圖、數銀、多房 | 永不 |

## 已知 bug 同陷阱

### v1 有、v1.5 修嘅版面 bug

1. **地圖被壓扁** — `.screen` 係 column flex，`#mapStage` 預設 `flex-shrink:1`，
   內容一超高度，`aspect-ratio` 就失效，地磚被拉成扁平條。修：`flex:0 0 auto`
   + 闊度上限改用 `min(560px, 92vw, calc(min(46vh,420px) * 1.2222))`，
   令高度被夾時闊度跟住縮，格仔維持正方形。
2. **清單被捲出畫面** — 矮視窗（筆電／分屏）會令頂部「清單」離開畫面，
   踩咗「清單常駐」紅線。修：`@media (max-height:820px)` 壓細方向掣。

### v1.6 修嘅 crash：形狀 mode 100% 死機

**症狀**：揀「中學·形」入地圖 → `TypeError: Cannot read properties of undefined (reading 'name')`
（`index.html` `updateHotspots` → `renderMap` → `startList`）。即係**一入形狀 mode 就死**。

**根因**：`listIds(list)` 喺形狀 mode 回傳嘅係**形狀名**（`["circle","triangle","square"]`），
唔係 item id。但 `renderMap()` 照攞 `listIds(L).concat(shelfIds(L))` 當 id 用 →
`"circle"` 被塞入 `shelfItems` → `ITEMS["circle"]` → `undefined` → `ITEMS[id].name` 讀唔到。

**修**：
```js
// 形狀 mode 全部實物都喺貨架，清單只描述形狀 → 只擺 shelfIds()
const ids = isShapeMode() ? shelfIds(L) : listIds(L).concat(shelfIds(L));
```
另外加咗兩個防禦性 guard：
- `updateHotspots()` — 未知 id 用 `ITEMS[id] ? … : "貨架"`，唔會再令成個地圖死機
- `setMode()` — 補 `state.stars = 0; state.log = [];`（切 mode 等於開新一節，唔應該帶住上一個 mode 嘅星星／記錄）

⚠️ **呢個係「唔好亂用 listIds()」嘅通用陷阱**：佢喺三個 mode 回傳三唔同語意嘅嘢
（形狀 mode = 形狀名 · 其他 = item id）。新增 mode 嗰陣，
`listIds()` 嘅每個 call site 都要重新睇。

### 仲未驗證嘅路徑
- **方向掣／鍵盤走位** — Browser 自動化工具送唔到具名按鍵（arrow key），所以
  「㩒格仔」＋「撳掣」路徑已實測，但「大掣 ▲▼◀▶ 逐步行」同「鍵盤 ↑↓←→」**未經自動化驗證**。
  落堂前人手試一次。

---

## 資料同私隱

- **唔寫 localStorage / sessionStorage** — 防止上一個學生嘅資料污染下一個學生。
- 老師記錄面板（右上「老師」掣）只存喺**記憶體**，老師用完**手動複製**走。
- 記錄欄位：模式 · 第幾張單 · 清單 · 攞到 · 漏咗 · 多咗 · 形狀跟單 · 多取 ·
  逐項核對 · 誠實答覆 · 實際 · 一致 · 付費 · 付足 · 獎

改 **logic／誠信規則** → 先改 `../value/`，再 sync 過來。  
改 **純視覺** → 只動本盤。

---

## Live

| | |
|---|---|
| **Repo** | https://github.com/ihateusingai-beep/sen-pixel-mart |
| **Pages** | https://ihateusingai-beep.github.io/sen-pixel-mart/ |

課堂主力 plain UI 仍係：https://ihateusingai-beep.github.io/sen-unmanned-store/  
**唔好**用本 repo 蓋過去。
