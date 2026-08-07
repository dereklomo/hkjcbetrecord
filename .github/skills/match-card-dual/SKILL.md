---
name: match-card-dual
description: 'Default pre-match workflow for this workspace: when the user pastes a football match card with odds, run defensive scoring under analysis-checklist v0.1 (U2 single 0–100 score from five equal dims: external, path, water, fundamentals, HK-external bucket; league tier label-only not in mean; tiers ≥60 actionable / ≥75 high-confidence / <60 PASS) and conditional streak-roll only when a ≥75 leg is draw-friendly (+0.25 or level). Use for 讓球 lines, 賽程+賠率, dual screen, 雙軌篩盤. Football only.'
argument-hint: 'Preferred: HKJC multi-line card (FBxxxx · 聯賽 · 時間 · 主 [盤] ： 客 [盤] · 主水 · 客水). Also one-line compact. Optional StreakState m/5 or 連勝.'
user-invocable: true
---

# Match Card Dual Screen（決策引擎 v0 · U2 分數）

## Purpose

**Default for this workspace** on schedule/card with odds:

1. **Defensive scoring（always）** — `defensive-betting-analysis` + `analysis-checklist.md` **v0**  
   - 每場 **一個 0–100 分**（F3 **五維均分** + O2 覆寫；**賽事檔不進均分**）  
   - **&lt;60 → PASS**（仍記分供校準）  
   - **≥60 → 可出手**（用戶自決是否下、下多少）  
   - **≥75 → 高信心標籤**（非注碼指令）  
   - **舊表 A / A′ / lean 入場標籤廢止**；命中帳 H1 凍結只讀  
2. **Streak-roll（conditional）** — `streak-roll-eval`  
   - 僅當存在 **≥75** 且盤 **draw-friendly**：**+0.25**（和=半贏）或 **平手**（和=P）  
   - 否則 **`streak: skipped`**

**第一 KPI（K5）：** 分數校準（分桶單調，含 PASS）。  
**副 KPI：** 事前分 ≥60 **且用戶實際有下** 的 Track B WR。  
**注碼：** 用戶全控；輸出 **N1**（無半注／滿注建議）。  
**cover 永不回寫。**

## When This Applies

- Odds paste / multi-match card / pre-match with prices  
- **Preferred：** HKJC Format A；**Also：** Format B compact / English dump  

**Skip dual pre-match for：** results-only；strategy-only；user `只 defensive` / `只 streak` / `不要連勝評級`.

## Read Order

1. This skill  
2. `analysis-checklist.md`（**§0 v0 分數引擎** · V1 · O2 · 輸出骨架）  
3. `defensive-betting-analysis`（Track B 硬結算 + Format A/B）  
4. `streak-roll-eval`（僅觸發時）  
5. `post-match-review-grok.md`（熱；舊 A/A′ **只讀**；新校準分桶）  
6. `post-match/INDEX.md` → **最近 2** batch  
7. `record/hk-external-gap-ledger.md`（**V5** 港外桶）

## Workflow

### Step 1 — Parse & identity

- Format A vs B（同前）  
- Format B：`主勝 主隊 主讓水 盤 客讓水 客隊 客勝`  
- Normalize 盤：`[0/-0.5]`→**−0.25**；`[0/+0.5]`→**+0.25**；`[-0.5/-1]`→**−0.75**；主 **+** = dog  
- **Track B 和局：** `0/-0.5` 和=**L**；`0/+0.5` 和=**半贏 W**；−0.75 和=**L**  
- Alias → 身份鎖；fail → `ID hold`（該場分=0，其餘繼續）  
- 命名外圍（可追溯數字）於 ID locked 後  

### Step 2 — Score every fixture（v0 核心）

對每場：

1. V1 硬否決？→ **分=0 → PASS**（見 checklist 0-V1）  
2. 否則 **五維** 各 0–100，**公式分 = round( (V1+V2+V3+V4+V5)/5 )**  
   - **V1** 外圍 · **V2** 港路徑 · **V3** 水位／膠著 · **V4** 基本面自行分析 · **V5** 港外桶  
   - **賽事／聯賽檔**（NPL／U20／盃等）→ **Why／標籤 only，不進均分**  
3. O2 覆寫（升 ≤+10；過 60 若公式&lt;60 須理由+`override`）  
4. 定檔：&lt;60 / ≥60 / ≥75（門檻 **不變**）  

**禁止：** 為填「可出手」硬抬分；incomplete 假 Med/Strong 過 60。

### Step 3 — Card assemble

- 可出手列表 = 所有 **最終分 ≥60**，**分降序**  
- PASS = 其餘（**仍輸出分**）  
- 不使用 表A／A′／lean 標籤  

### Step 4 — Streak（conditional）

| 條件 | 動作 |
|------|------|
| 有 **≥75** 且盤為 **+0.25** 或 **平手** | 跑 streak-roll-eval |
| 其他 | **`streak: skipped`** |
| 用戶強制 streak | 可跑 |

### Step 5 — Response skeleton（v0 · 強制）

```markdown
# Dual Card Screen

## 可出手（分數 ≥60 · 降序）
| # | Match | Market | 分 | 檔 | Why（維度要點；override 必標） |
|---|-------|--------|----|----|--------------------------------|
| 1 | … | … | 78 | ≥75 | … |
（無 → **none**）

## PASS（分 &lt;60 或 V1 否決）
| 場 | 分 | 理由 |
|----|----|------|
| … | 42 | V3 高水半一 / 否決#3 深讓 … |

## Streak-roll
Status: evaluated | **skipped** (reason)

## One-line
可出手: n | 頂分: Match 分 | ≥75: n | PASS: n | Streak: …

## Exec Gate（強制）
- [ ] 主勝/客勝已對欄
- [ ] 外圍：已查 **或** incomplete + Sources none
- [ ] 凡 ≥60 → 命名 Sources + 數字
- [ ] V1 / 水位膠著精神 / 路徑和局含義已套
- [ ] 分數表 + PASS + One-line + Streak 齊
```

### 輸出規則

1. **可出手表** 僅 ≥60；**分降序**。  
2. **PASS 必含分**（K5 校準）。  
3. **不輸出注碼**（半注／滿注／單位）。  
4. 全 PASS → 可出手 **none** + PASS 表填滿。  
5. Gate 未過 → **禁止** 定 ≥60。

### 賽後（有賽果時）

```markdown
## 校準反省
- 預測 vs 賽果：（對齊 **分數／檔**；高分 L／低分 cover）
- 建議：（≥1 條；可提調權／否決）
```

- batch §9；INDEX；港外分差 append（出手或關鍵結構）  
- **不** append 舊表 A／A′ 命中列（H1 凍結）  
- 副 WR：僅 **事前≥60 且用戶確認有下**（若未知是否有下，batch 標「結構可出手」勿冒充下單帳）

## Rules of Combination

1. 全 PASS OK  
2. 多腳 ≥60 OK  
3. 不發明垃圾邊灌分  
4. cover after PASS 不回寫  
5. V1 否決不可覆寫救活  
6. 注碼用戶自有  
7. 舊 A/A′/lean 標籤不使用  

## Quality Checks

- [ ] 每場有最終分（或 V1=0）  
- [ ] ≥60 有外圍數字或明示結構例外+低外圍分  
- [ ] 基本面有自析或 unknown  
- [ ] 無表A/A′/lean 入場標籤  
- [ ] 無注碼建議  
- [ ] `0/-0.5` 未誤標和=W  
- [ ] Exec Gate 五勾誠實  
- [ ] 輸出骨架完整  

## Post-match

Results → batch + INDEX + 校準反省（分／檔）+ 港外分差；**不**續寫凍結的表 A/A′ 主分母。  
Do **not** load full legacy by default.

## Do not archive user paste

- 無用戶明示不寫 raw paste 進 selfnote／pre-match  
- 預設 = 分數決策 only  
