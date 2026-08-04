---
name: match-card-dual
description: 'Default pre-match workflow for this workspace: when the user pastes a football match card with odds, run defensive screening under analysis-checklist (table-A whitelist formal PLAYs + quasi-table A′ soft PLAY only if table A empty) and run streak-roll-eval only when a table-A formal is draw-friendly (-0.25 or level-ball lean). Use for 讓球 lines, 賽程+賠率, dual screen, 雙軌篩盤. Football only.'
argument-hint: 'Preferred: HKJC multi-line card (FBxxxx · 聯賽 · 時間 · 主 [盤] ： 客 [盤] · 主水 · 客水). Also one-line compact. Optional StreakState m/5 or 連勝.'
user-invocable: true
---

# Match Card Dual Screen (Defensive + Conditional Streak)

## Purpose

**Default for this workspace** on schedule/card with odds:

1. **Defensive (always)** — `defensive-betting-analysis` + `analysis-checklist.md`  
   - **Formal PLAY (table A):** whitelist **W1–W6** only; **0 / 1 / many** legs; **no** §1-max  
   - **準表 A′ (Soft PLAY):** **only if table A is empty** — **0–1** leg; **small stake OK**; **not** table A WR; post-match → **table A′**  
   - Pure **Max-WR / Obs column is retired** (merged into A′)  
2. **Streak-roll (conditional)** — `streak-roll-eval`  
   - Run **only if** table A has a **draw-friendly** formal: **-0.25** or **level-ball lean**  
   - Else **`streak: skipped`** (unless user forces 連勝/streak)

**Primary goal (grill-locked):** maximize **justified table-A Track B win-rate**.  
**Frequency valve:** A′ when table A empty — does **not** relax table-A triple gate.

## When This Applies

- Odds paste / multi-match card / pre-match with prices  
- **Preferred paste:** HKJC **card block** (`FBxxxx` + 中文聯賽/隊名 + `[盤]` + 兩側讓球水) — see `defensive-betting-analysis` → **Preferred Match Input Format · Format A**  
- **Also accept:** one-line compact / English dumps (Format B)

**Skip dual pre-match for:** results-only; strategy-only; user `只 defensive` / `只 streak` / `不要連勝評級`.

## Read Order

1. This skill  
2. `analysis-checklist.md` (**§1-A / §1-A′ / §1-streak**, whitelist W1–W6)  
3. `defensive-betting-analysis` (Track B hard rules + **input format A/B**)  
4. `streak-roll-eval` (only when triggered)  
5. `post-match-review-grok.md` (**hot** ledgers only: table A / **A′** / branch WR)  
6. `post-match/INDEX.md` → **latest 2** batch files under `post-match/batches/` (not full legacy)

## Workflow

### Step 1 — Parse & identity

- Detect format: **`FBxxxx` / 中文聯賽+`[ 盤 ]`+`：`** → **Format A (HKJC card block · preferred)**; else one-line → Format B  
- Unlabeled waters = **HKJC** execution (AH prices; often **no 1X2** in Format A)  
- **Format B 7 欄：** `主勝 主隊 主讓水 盤 客讓水 客隊 客勝` — **欄1≠欄7**；熱門寫 **`主勝 X` 或 `客勝 Y`**，禁止「較短獨贏=主勝」捷徑  
- Normalize: home first; `[ -0.5 / -1 ]` → **-0.75**; `[ 0 / -0.5 ]` → **-0.25**; `[ +1 / +1.5 ]` → **+1.25**; `[ 0 ]` → level；**home + = AH dog**  
- Format A: first water = **home AH**, second = **away AH**（無 1X2 時勿捏造主勝）  
- **Alias first:** read `record/hkjc-name-alias.md` (if present) before web search; use `FBxxxx` as key when useful  
- Lock identity: **teams + competition + card date** (+ **home/away** if high-risk — see defensive skill Identity Gate)  
- **If English names / fixture do not lock → that fixture `ID hold`:** ask user (1–2 candidates); **no external attach**; **rest of card continues**  
- **Do not** fail identity because external 1X2 ≠ HKJC AH water (HKJC = execution only)  
- Seek **named** external 1X2 with **traceable numbers** only after `ID locked` (table-A formals need this; Format A usually lacks ML)  
- Label fixtures: `ID locked` | `ID hold` | `incomplete` (no external)  
- Same-day **re-paste** = **latest execution**; note deltas  
- Multi-paste same date = card a/b/c until user asks **合併一檔**

### Step 2 — Score every fixture

For each: external gap, Track B map, fundamentals (incl. **draw-magnet / cagey?**), league tier A/B/C/D, branch tag.

### Step 3 — Table A formal (whitelist)

Promote to **Formal PLAY (table A)** only if:

- Ideal §1 gates (named external, Medium as required, hard vetoes clear)  
- Branch ∈ **W1–W6**  
- **Not** draw-magnet/cagey **-0.75**  
- **Not** two-legged euro  
- Single-leg cup only if Strong (≤~1.55) + shallow  
- **Not** cup dog **+1** as table A  

**Multiple** table-A legs OK if each qualifies; **no same-branch parlay**.  
**Forbidden:** §1-max forcing non-whitelist path into table A.

### Step 4 — 準表 A′ (only if table A empty) · **engine fix Grill 08**

**Section always present** (write **none** if not used — **empty A′ is OK and common**).

If **zero** table-A formals:

**Do NOT** pick pure path-wide dog (+1/+1.25) as Soft just because path is widest.

**A′ only if all hold:**

1. **Named external with numbers** (incomplete → no A′ Soft)  
2. **Fundamentals** hard vetoes clear; draw-magnet **-0.75** → ban A′  
3. Market is either:  
   - **Shortfall favorite/shallow:** **-0.25** / level lean / **-0.75** (near W1–W6 or W7 C-tier; named external)  
   - **OR euro-2leg dog +1/+0.75** only (Strong fav labeled + blast Risk)  
4. **Banned from A′ (lean-only WR watch):** **league dog +1/+1.25**, **NPL/semi dog +1/+1.25**, **cup dog +1**, any dog with fav ML **≤1.55**  
5. Among valid candidates: prefer **shortfall shallow/fav** over euro dog; **never** rank by path width alone  
6. **Four sentences required** on any A′ Soft: external · fundamentals · why not table A · why dog allowed/banned  
7. Label **`Soft PLAY / 準表 A′ — NOT table A`**; post-match → **table A′** only  

If table A non-empty **or** no valid candidate: **A′ = none**.

### Step 5 — Streak (conditional)

| Table A content | Streak section |
|-----------------|----------------|
| Has **-0.25** or **level-ball lean** formal | Run streak-roll-eval |
| Only **-0.75** / **+1** / empty | **`streak: skipped`** |
| User forces streak | Run eval anyway |

### Step 6 — Response skeleton

```markdown
# Dual Card Screen

## A. Defensive (WR-first · table A priority)
### Formal PLAY（表 A · W1–W6 · 可多腳 · 命中帳）
| # | Match | Market | WL | Conf | Why |
（or: **none**）

### 準表 A′（可小注 · 不進表 A · 僅表 A 空 · 0～1 腳）
| Match | Market | Why A′ | Tag | Risk |
（or: **none** — table A non-empty / no valid shortfall or euro-2leg dog）
**四句（有 A′ 必填）：** 外圍 / 基本面 / 為何非表 A / 為何允許或禁止 dog

### WR lean only
...

### Rest: PASS
...

## B. Streak-roll
StreakState: m/5
Status: evaluated | **skipped** (reason)
### Best next STREAK_LEG
...

## C. One-line
Table A: … | A′: … | Streak: …

## Exec Gate（強制 · 不可省略）
- [ ] 主勝/客勝已對欄（Format B 欄1/欄7；或 Format A 無 ML 已標）
- [ ] 外圍：已查 **或** incomplete + `Sources Used: none`
- [ ] 凡 Med/Strong/方向 lean/A′/表 A → 有**命名 Sources + 數字**（否則只能結構 PASS）
- [ ] §1b-2 / §1-A′-dog（若相關）已套
- [ ] 表 A / A′ / Streak 三欄齊
```

### Exec Gate 規則（Grill 2026-08 · 防執行疏失）

1. **每張** dual／賽前卡結尾 **必須** 有 `## Exec Gate` 五勾（可 `[x]`／`[ ]` 如實）。  
2. **任一未過** → **禁止** 定 `PLAY`／**A′ Soft**／帶力度分檔的方向 lean；允許：結構 PASS 清單、incomplete、GATE 缺項說明。  
3. 不得「先出完整 dual 再補 Gate」當過關；Gate 是輸出的一部分。  
4. 外圍漏跑 = Gate #2/#3 必須為未勾或 incomplete，不得假裝已查。

## Rules of Combination

1. Empty table A is OK  
2. Many table-A legs OK if each ∈ W1–W6  
3. Do not invent table-A outside W1–W6  
4. Do not put A′ into table A WR  
5. Do not invent garbage sides  
6. Cover after PASS/lean never rewrites to table A  
7. A′ only when table A empty (0–1 leg)  
8. Streak empty/skipped OK  
9. Fund management user-owned  

## Quality Checks

- [ ] Table A legs each have Whitelist ID W1–W6  
- [ ] No §1-max non-whitelist formal  
- [ ] No draw-magnet **-0.75** in table A or A′  
- [ ] No two-legged euro / cup dog+1 in **table A**  
- [ ] A′ section present; only when table A empty; **none OK**  
- [ ] A′ not pure path-wide league/NPL dog; euro-2leg dog only exception  
- [ ] A′ has external numbers + fundamentals + **four sentences**  
- [ ] D-tier A′ (if any) shallow + named external + sub-ledger only  
- [ ] Streak skipped unless draw-friendly table A (or force)  
- [ ] Named sources on table A formals (real Sources or `none`; no fake lean)  
- [ ] Identity status respected (`ID hold` → no external-based PLAY/A′)  
- [ ] **主勝/客勝 not swapped** on Format B; gap tier labels home vs away correctly  
- [ ] **`## Exec Gate` present** with five checks honest; no PLAY/A′/tiered lean if gate fails  
- [ ] Fundamentals present  

## Post-match

Results-only → append **hot** table A / **A′** + branch quick-ref; write **`post-match/batches/<date>.md`** (§9); update **`post-match/INDEX.md`**.  
Do **not** load `2026-07-archive-legacy.md` by default.  
Do not rewrite historical PASS into table A after cover.

## Do not archive user paste (workspace · 2026-08)

- **Do not** append raw user odds paste to `selfnote.txt`, pre-match「原始盤」blocks, or other record files **unless the user explicitly asks** to save/write/merge a file.  
- Default response = **decisions only** (table A / A′ / lean / PASS / one-line).  
- Pre-match files only when user requests write-in; prefer **decision summary**, not full paste dump.
