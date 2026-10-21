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
- GBC 色板、直角、硬陰影、scanline、GB 外框
- 文案 PRESS START / PIXEL MART
- **未做**：自由 roam、真銀找續、多房探索（刻意唔做）

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
| **v0** | 複製原版骨 + 8-bit CSS 皮 | **而家** |
| v1 | 貨架／櫃檯簡單 pixel 場景底圖（仍大掣點選） | 待 |
| v1.5 | 一掣「去櫃檯」短過場（自動，唔探索） | 待 |
| ❌ | 自由地圖、數銀、對戰選單 | 永不（中度） |

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
