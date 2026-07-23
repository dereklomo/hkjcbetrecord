---
name: match-card-dual
description: 'Default pre-match workflow for this workspace: whenever the user pastes a football match card with odds (HKJC-style lines, multi-match batch, or schedule+prices), run BOTH defensive-betting-analysis (best ordinary PLAY / lean / PASS) and streak-roll-eval (best STREAK_LEG for full-equity 5-win roll, if any). Use when user posts 讓球 lines with prices, 賽程+賠率, multi-line odds paste, pre-match card, or asks for dual screen / 雙軌篩盤. Football only.'
argument-hint: 'Paste one or more lines: 勝賠 隊A 讓賠 盤 受賠 隊B 勝賠 (HKJC default). Optional StreakState m/5.'
user-invocable: true
---

# Match Card Dual Screen (Defensive + Streak-Roll)

## Purpose

**Default for this workspace:** every time the user inputs a **schedule/card with odds**, produce **both**:

1. **Defensive** (`defensive-betting-analysis` + `analysis-checklist.md`) — best ordinary formal `PLAY`(s), WR lean-only, and PASS screen  
2. **Streak-roll** (`streak-roll-eval`) — best **next** `STREAK_LEG` candidate for full-equity sequential roll toward **5 wins**, or explicit **none**

Do **not** skip streak-roll because “user didn’t ask for all-in.” Dual output is the default unless the user says **only defensive** / **only streak** / **post-match only**.

## When This Applies

Trigger on **any** of:

- Multi-line or single-line paste matching HKJC/odds format  
- Dated card (`20260724` + lines)  
- “分析” / “篩盤” / “pre-match” with prices  
- Implicit batch of 讓球/受讓/平手 with numbers  

**Do not** force dual mode for:

- Pure post-match results (use defensive post-match + ledger append)  
- Pure strategy/meta questions without a card  
- User explicitly: `只 defensive` / `只 streak-roll` / `不要連勝評級`

## Read Order (this repo)

1. This skill (orchestration)  
2. `analysis-checklist.md`  
3. `.github/skills/defensive-betting-analysis/SKILL.md` (hard WR rules)  
4. `.github/skills/streak-roll-eval/SKILL.md` (streak gates)  
5. `post-match-review-grok.md` ledgers A/B for cross-check  

## Workflow (fixed)

### Step 1 — Parse & identity

- Parse all lines; treat unlabeled as **HKJC**  
- Lock identities; ask user if uncertain (blocks PLAY and STREAK_LEG)  
- Seek external 1X2 (named sources) before directional claims  

### Step 2 — Defensive pass (every fixture)

For each match apply defensive + checklist:

- Identity + external + **fundamentals modifier** (motivation, personnel, travel, rotation, season phase)  
- `PLAY` | `WR lean only` | `PASS`  
- Formal PLAY only if checklist §1 (and §1b if `-0.75`) **and** fundamentals do not kill the WR case  
- Collect formal PLAYs and notable leans  
- **Anti-miss (§1d):** after the batch, scan for (1) clear-league fav **`-0.25`**, (2) structural dog **`+0.25` / `+1`**, (3) level-ball + external lean — if all gates pass but output was PASS/lean only, **upgrade to formal Low PLAY**. Empty scan = OK. Do **not** open light / NPL / thin `+0.75`.

### Step 3 — Streak-roll pass (every fixture)

For each match apply streak-roll-eval (stricter):

- Default `NOT_ELIGIBLE`  
- **Odds + Track B path + fundamentals** all required; fundamentals veto (rotation / dead rubber / key absences) blocks STREAK_LEG even if 1X2 looks strong  
- `STREAK_LEG` only if all streak gates pass  
- Ordinary defensive `PLAY` **≠** auto `STREAK_LEG`  
- Hostile: `-0.75` (draw=L), pure `-1`, deep fav, thin dog, light `-0.25`, NPL/USL, euro Q first leg, near-even  

### Step 4 — Pick “best of”

| Track | How to pick “best” |
|-------|---------------------|
| **Defensive** | All formal `PLAY`s (small-stake); if zero, top 1–3 **WR lean only** (labeled not bets); if none, state **no defensive PLAY** |
| **Streak-roll** | At most **one** next `STREAK_LEG` (or zero). Prefer draw-friendly paths (`-0.25` clear lean, level-ball clear lean). Never package 5 simultaneous all-in legs |

If both tracks highlight the **same** match: say so, and warn streak rules are **stricter** (may still be NOT_ELIGIBLE for roll while PLAY for unit defensive).

### Step 5 — Required response structure

Always use this skeleton for odds-card input:

```markdown
# Dual Card Screen

## A. Defensive (unit / WR-first)
### Formal PLAY
| # | Match | Market | Price | Conf | Why (WR + fundamentals) |
...
(or: none)

### WR lean only (not bets)
...

### Rest: PASS (count or short groups)

### 防漏升掃描 (checklist §1d — required)
| 結構 | 本卡候選 | 結果 |
|------|----------|------|
| clear 聯賽 -0.25 | … / 無 | PLAY / 否決 / 無候選 |
| 結構 dog +0.25 | … / 無 | … |
| 結構 dog +1 或 平手 lean | … / 無 | … |

## B. Streak-roll (full-equity · target 5 wins)
StreakState: m/5 (default 0/5 if not given)

### Best next STREAK_LEG
(or: **none — NOT_ELIGIBLE all**)

[If STREAK_LEG: full streak block — must include Fundamentals snapshot + veto Y/N]

### Why others fail streak (short)
- hostile path / light / cup first leg / deep / **fundamentals veto** / etc.

## C. One-line summary
Defensive: ... | Streak-roll: ...
```

**Fundamentals are required on both tracks** — not odds-only screening.

## Rules of Combination

1. **Empty is OK** on either side — especially streak  
2. **Do not** invent a STREAK_LEG to “fill” section B  
3. **Do not** upgrade defensive PASS to PLAY to fill section A  
4. Fund management stays **user-owned** (no stake amounts for full-roll)  
5. Defensive PLAY still **Low + 小注** per checklist; streak section does not change that  
6. Same-night multiple defensive PLAYs: still **no same-branch parlay**; streak still **one next leg max**  

## Quality Checks

- [ ] Both sections A and B present  
- [ ] Streak section not skipped on odds card  
- [ ] Sources named when external used  
- [ ] **Fundamentals** considered on PLAY and STREAK (not odds-only)  
- [ ] `-0.75` formal defensive states **draw=L**  
- [ ] Streak does not recommend `-0.75` as default STREAK_LEG  
- [ ] Identity holds asked before any PLAY/STREAK_LEG  
- [ ] **防漏升 §1d** scanned (clear league `-0.25` / structural dog `+0.25`/`+1` / level-ball lean); no silent PASS on full-gate spots  
- [ ] Did **not** “anti-miss” into light `-0.25`, NPL, or thin dog `+0.75`

## Post-match exception

If input is **only results** (scores, no new pricing card): skip dual pre-match; run post-match review / ledger update under defensive skill.  
If input is **results + “update streak chain”**: update streak chain state (W/L/P → m/5 or BROKEN) briefly, still no dual pre-match unless a new card is attached.
