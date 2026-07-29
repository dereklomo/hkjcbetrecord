---
name: match-card-dual
description: 'Default pre-match workflow for this workspace: whenever the user pastes a football match card with odds (HKJC-style lines, multi-match batch, or schedule+prices), run BOTH defensive-betting-analysis (best ordinary PLAY / lean / PASS) and streak-roll-eval (best STREAK_LEG for full-equity 5-win roll, if any). Use when user posts 讓球 lines with prices, 賽程+賠率, multi-line odds paste, pre-match card, or asks for dual screen / 雙軌篩盤. Football only.'
argument-hint: 'Paste one or more lines: 勝賠 隊A 讓賠 盤 受賠 隊B 勝賠 (HKJC default). Optional StreakState m/5.'
user-invocable: true
---

# Match Card Dual Screen (Defensive + Streak-Roll)

## Purpose

**Default for this workspace:** every time the user inputs a **schedule/card with odds**, produce **both**:

1. **Defensive** — **§1-max one max-WR formal PLAY** (required if any edge) + leans/PASS  
2. **Streak-roll** — best **STREAK_LEG** or **none** (still strict; empty OK)

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

- Identity + external + **fundamentals modifier**  
- Score each fixture on **Track B WR** (path + external gap + league tier + fundamentals)  
- Apply A–E as **ranking penalties / Risk notes**, **not** card-wide hard zeros (see checklist §1-max)  
- Tag league tier **A/B/C/D**  
- Collect candidates for formal / lean / PASS  

### Step 2b — §1-max: one best WR formal PLAY (required)

After all fixtures:

1. Rank candidates by Track B win-rate (path friendliness first: e.g. `+1` draw=W / lose-by-1=P often beats `-0.75` draw=L when edges similar)  
2. Output **exactly one** primary formal `PLAY` = **max WR on this card** (Low stake), unless *every* fixture is true coin-flip / identity-broken / unreadable  
3. May add other ideal §1 PLAYs; still **must** label the **§1-max 主推**  
4. Do **not** leave formal empty when any usable WR edge exists  
5. Do **not** invent a reverse/garbage side just to fill the slot  

**Anti-miss (§1d):** still upgrade full-gate clear `-0.25` / clean dog structures; empty anti-miss is OK if §1-max already picked the best path.

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
| **Defensive** | **§1-max:** exactly **one** max-WR formal `PLAY` (required if any edge). Optional extra ideal §1 PLAYs. Then leans / PASS |
| **Streak-roll** | At most **one** next `STREAK_LEG` (or zero). Prefer draw-friendly paths. Empty OK |

If both tracks highlight the **same** match: say so, and warn streak rules are **stricter** (may still be NOT_ELIGIBLE for roll while PLAY for unit defensive).

### Step 5 — Required response structure

Always use this skeleton for odds-card input:

```markdown
# Dual Card Screen

## A. Defensive (unit / WR-first)
### Formal PLAY（§1-max 本卡最大 WR · 主推 1 腳）
| # | Match | Market | Price | Conf | WR 排序理由 |
| 1 | … | … | … | Low | 路徑 > 外圍 > 分檔 |

### WR lean only (not bets)
...

### Rest: PASS (count or short groups)

### 防漏升掃描 (checklist §1d)
...

## B. Streak-roll (full-equity · target 5 wins)
StreakState: m/5
### Best next STREAK_LEG
(or: **none**)

## C. One-line summary
Defensive: §1-max … | Streak-roll: …
```

**Fundamentals on both tracks.** A–E = rank/risk modifiers, not card-wide formal ban.

## Rules of Combination

1. **Streak empty OK**; defensive formal empty **only** if no usable WR edge on entire card  
2. **Do not** invent STREAK_LEG to fill B  
3. **Do** pick **max WR** formal when edges exist; **do not** invent garbage reverse sides  
4. Fund management **user-owned**  
5. Defensive **Low + 小注**; no same-branch parlay; streak one next leg max  

## Quality Checks

- [ ] Both A and B present  
- [ ] **§1-max one primary formal PLAY** when any usable edge exists  
- [ ] Why this leg beats others on **Track B path** stated  
- [ ] Streak may be none  
- [ ] Strong `+1` / draw-magnet / incomplete: Risk noted if §1-max  
- [ ] No reverse/garbage filler

## Post-match exception

If input is **only results** (scores, no new pricing card): skip dual pre-match; run post-match review / ledger update under defensive skill **and** checklist **§9 Skill 嚴寬簡審** (mandatory short table every results batch).  
If input is **results + “update streak chain”**: update streak chain state (W/L/P → m/5 or BROKEN) briefly, still no dual pre-match unless a new card is attached; still include §9 if ledgers were updated.
