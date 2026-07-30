---
name: match-card-dual
description: 'Default pre-match workflow for this workspace: when the user pastes a football match card with odds, run defensive screening under analysis-checklist (table-A whitelist formal PLAYs + max-WR observation only if table A empty) and run streak-roll-eval only when a table-A formal is draw-friendly (-0.25 or level-ball lean). Use for 讓球 lines, 賽程+賠率, dual screen, 雙軌篩盤. Football only.'
argument-hint: 'Paste HKJC-style lines. Optional StreakState m/5 or 連勝 to force streak eval.'
user-invocable: true
---

# Match Card Dual Screen (Defensive + Conditional Streak)

## Purpose

**Default for this workspace** on schedule/card with odds:

1. **Defensive (always)** — `defensive-betting-analysis` + `analysis-checklist.md`  
   - **Formal PLAY (table A):** whitelist **W1–W6** only; **0 / 1 / many** legs (each must qualify; **no** §1-max one-forced-play)  
   - **Max-WR observation (Obs):** **only if table A is empty** — 0–1 path-best leg; **not** table A; post-match → **table Obs** ledger  
2. **Streak-roll (conditional)** — `streak-roll-eval`  
   - Run **only if** table A has a **draw-friendly** formal: favorite **-0.25** (draw=W) or **level-ball lean** (draw=P)  
   - Otherwise output **`streak: skipped`** (unless user forces 連勝/streak)

**Primary goal (grill-locked):** maximize **justified table-A Track B win-rate**, not “always have a bet.”

## When This Applies

- Odds paste / multi-match card / pre-match with prices  

**Skip dual pre-match for:** results-only; strategy-only; user `只 defensive` / `只 streak` / `不要連勝評級`.

## Read Order

1. This skill  
2. `analysis-checklist.md` (**§1-A / §1-obs / §1-streak**, whitelist W1–W6)  
3. `defensive-betting-analysis` (Track B hard rules)  
4. `streak-roll-eval` (only when triggered)  
5. `post-match-review-grok.md` ledgers  

## Workflow

### Step 1 — Parse & identity

- Parse lines; unlabeled = **HKJC**  
- Lock identity; ask if uncertain  
- Seek **named** external 1X2  

### Step 2 — Score every fixture

For each: external gap, Track B map, fundamentals (incl. **draw-magnet / cagey script?**), league tier A/B/C/D, branch tag.

### Step 3 — Table A formal (whitelist)

Promote to **Formal PLAY (table A)** only if:

- Ideal checklist §1 gates (named external, Medium as required, fundamentals hard vetoes clear)  
- Branch ∈ **W1–W6**  
- **Not** draw-magnet/cagey **-0.75**  
- **Not** two-legged euro  
- Single-leg cup only if Strong (≤~1.55) + shallow line  
- **Not** cup dog **+1** as table A (lean/Obs only)

**Multiple** table-A legs allowed if each qualifies; **no same-branch parlay**.  
**Forbidden:** §1-max forcing one non-whitelist “best path” into table A (e.g. euro Strong **+1**).

### Step 4 — Max-WR observation (only if table A empty)

**Section always present** in the card (write **none** if not used).

If **zero** table-A formals and some usable edge exists:

- Rank by **path width:** dog **+1** (W/draw=W, lose-1=P) **>** clear **-0.25** / level lean **>** **-0.75** (draw=L)  
- **C-tier -0.75** allowed only if no wider path on the card  
- **League/cup/euro all allowed** for Obs; tag `league` / `cup` / `euro-2leg` / `Strong-blast`  
- **D-tier** (women / U20 / semi-pro) **not** Obs main column  
- Hard vetoes still apply (unlocked identity, true 50-50, B-team / dead rubber)  
- Pick **one** leg; label **`Max-WR Observation — NOT table A`**  
- Post-match: append **table Obs** (never merge WR into table A)

If table A non-empty: **Obs = none** (do not force a second column bet).

### Step 5 — Streak (conditional)

| Table A content | Streak section |
|-----------------|----------------|
| Has **-0.25** or **level-ball lean** formal | Run streak-roll-eval → STREAK_LEG or none |
| Only **-0.75** / **+1** / empty | **`streak: skipped`** |
| User forces streak | Run eval anyway |

### Step 6 — Response skeleton

```markdown
# Dual Card Screen

## A. Defensive (WR-first · table A priority)
### Formal PLAY（表 A · 白名單 W1–W6 · 可多腳 · 可進命中帳）
| # | Match | Market | WL | Conf | Why |
（or: **none**）

### Max-WR 觀察（表 Obs · 不進表 A · 僅表 A 空時 · 欄位必有）
| Match | Market | Path rank | Tag | Risk |
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
Table A: … | Observation: … | Streak: …
```

## Rules of Combination

1. **Empty table A is OK** and often correct  
2. **Many table-A legs OK** if each ∈ W1–W6  
3. **Do not** invent table-A PLAY outside W1–W6  
4. **Do not** put Max-WR / Obs into table A  
5. **Do not** invent garbage sides to fill either column  
6. Cover after PASS/lean **never** rewrites to table A  
7. Streak empty/skipped OK  
8. Fund management user-owned  

## Quality Checks

- [ ] Table A legs each have Whitelist ID W1–W6  
- [ ] No §1-max single forced formal outside whitelist  
- [ ] No draw-magnet **-0.75** in table A  
- [ ] No two-legged euro / cup dog+1 in table A  
- [ ] Max-WR section present; Obs only when table A empty  
- [ ] Obs path rank respected; D-tier not Obs main  
- [ ] Streak skipped unless draw-friendly table A (or user force)  
- [ ] Named sources on table A formals  
- [ ] Fundamentals present  

## Post-match

Results-only → ledger append + checklist **§9** (Lean short table + strict/loose).  
Do not rewrite historical PASS into table A after cover.
