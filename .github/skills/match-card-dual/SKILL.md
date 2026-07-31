---
name: match-card-dual
description: 'Default pre-match workflow for this workspace: when the user pastes a football match card with odds, run defensive screening under analysis-checklist (table-A whitelist formal PLAYs + quasi-table A′ soft PLAY only if table A empty) and run streak-roll-eval only when a table-A formal is draw-friendly (-0.25 or level-ball lean). Use for 讓球 lines, 賽程+賠率, dual screen, 雙軌篩盤. Football only.'
argument-hint: 'Paste HKJC-style lines. Optional StreakState m/5 or 連勝 to force streak eval.'
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

**Skip dual pre-match for:** results-only; strategy-only; user `只 defensive` / `只 streak` / `不要連勝評級`.

## Read Order

1. This skill  
2. `analysis-checklist.md` (**§1-A / §1-A′ / §1-streak**, whitelist W1–W6)  
3. `defensive-betting-analysis` (Track B hard rules)  
4. `streak-roll-eval` (only when triggered)  
5. `post-match-review-grok.md` (**hot** ledgers only: table A / **A′** / branch WR)  
6. `post-match/INDEX.md` → **latest 2** batch files under `post-match/batches/` (not full legacy)

## Workflow

### Step 1 — Parse & identity

- Parse lines; unlabeled = **HKJC**  
- Lock identity; ask if uncertain  
- Seek **named** external 1X2  

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

### Step 4 — 準表 A′ (only if table A empty)

**Section always present** (write **none** if not used).

If **zero** table-A formals and usable edge:

- **One-leg shortfall** OR **structure pack** (euro dog +1/+0.75, cup +1, C-tier -0.75, …)  
- **Path rank:** dog **+1/+0.75** > **-0.25**/level > **-0.75**  
- Draw-magnet **-0.75** → **ban A′**  
- **D-tier** (women/U20/semi): allowed only in **sub-ledger** with **shallow line + named external**; never mix WR with league/euro A′  
- Label **`Soft PLAY / 準表 A′ — NOT table A`**  
- Post-match: append **table A′** only  

If table A non-empty: **A′ = none**.

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
| Match | Market | Why A′ | Tag / 子帳 | Risk |
（or: **none** — table A non-empty / no edge / veto）

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
```

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
- [ ] A′ section present; only when table A empty  
- [ ] A′ path rank + shortfall/structure pack respected  
- [ ] D-tier A′ has sub-ledger + shallow + named external  
- [ ] Streak skipped unless draw-friendly table A (or force)  
- [ ] Named sources on table A formals  
- [ ] Fundamentals present  

## Post-match

Results-only → append **hot** table A / **A′** + branch quick-ref; write **`post-match/batches/<date>.md`** (§9); update **`post-match/INDEX.md`**.  
Do **not** load `2026-07-archive-legacy.md` by default.  
Do not rewrite historical PASS into table A after cover.
