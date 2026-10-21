# PIXEL MART scope

## 鎖定
- 教學流程 = `value/` 原版（對貨 + 櫃檯付足）
- 操作 = 大掣／點圖，唔使十字走位
- 比錢 = 符號三選，唔數銀

## 禁止
- open-world 超市
- 多層選單／背包
- 即時 NPC 劇透對錯
- 覆蓋 `sen-unmanned-store` Live

## 同步
原版 logic 修好 → diff 進本盤 JS；CSS 皮保留本盤。

## 皮／骨分界（v1.5 重繪之後）

v1.5 換咗成個地圖嘅 **render 層**（canvas pixel-art），但**教學邏輯一行冇改**：
`hotspotNear()` / `pickItemFromMap()` / `doInteract()` / `goToCheckout()` 全部原封不動。

| 屬邊 | 內容 |
|---|---|
| 骨（唔准喺本盤亂改） | 清單 → 核對 → 對貨 → 三選付費 → 星星條件 |
| 皮（隨時可換） | `PAL` 色板 · `drawKid` · `drawFloor` · `drawShelf` · `drawCounter` · `drawDoor` · `drawBang` |

兩個開關：
- `AUTO_WALK`（`true`）— 㩒遠格自動行過去。**只行唔互動**：攞唔攞一定要學生自己撳，
  自動代佢決定會直接破壞誠信學習目標。
- `MAP_LAYOUT` — 11×9 字串地圖。`#` 牆 · `.` 地 · `S` 架 · `C` 櫃檯 · `D` 門。
