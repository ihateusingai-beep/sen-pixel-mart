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

## 開機

```bash
open "index.html"   # 雙擊即開
```

| 檔 | 用途 |
|---|---|
| `index.html` | 單檔教材 |
| `assets/puichi-logo.png` | 校徽（離線） |
| `docs/` | 規格／演進筆記 |

---

## 路線圖（鎖死）

| 版 | 做 | 狀態 |
|---|---|---|
| **v0** | 複製原版骨 + 8-bit CSS 皮 | 完成 |
| **v1–v2** | 單屏地圖 · 角色格移 · 貨架／櫃檯熱點 · 去櫃檯捷徑 · 接 check/pay | 完成 `a348bf1` |
| **v1.5** | Pixel-art canvas 重繪（sprite / 動畫 / 浮「！」）+ 版面 bug 修復 | **而家** |
| **v2.5** | 模式 A 自動行路（㩒遠格 BFS 走过去） | 完成（`AUTO_WALK` 開關） |
| ❌ | 自由地圖、數銀、多房 | 永不 |

## 兩個版面 bug（v1 有、v1.5 修）

1. **地圖被壓扁** — `.screen` 係 column flex，`#mapStage` 預設 `flex-shrink:1`，
   內容一超高度，`aspect-ratio` 就失效，地磚被拉成扁平條。修：`flex:0 0 auto`
   + 闊度上限改用 `min(560px, 92vw, calc(min(46vh,420px) * 1.2222))`，
   令高度被夾時闊度跟住縮，格仔維持正方形。
2. **清單被捲出畫面** — 矮視窗（筆電／分屏）會令頂部「清單」離開畫面，
   踩咗「清單常駐」紅線。修：`@media (max-height:820px)` 壓細方向掣。

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
