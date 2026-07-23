---
name: streak-roll-eval
description: 'Evaluate football handicap spots for full-equity sequential roll (all bankroll+profit on next leg) with a default target of 5 consecutive Track-B wins. Built on the same identity, external-odds, and half-win=W / half-loss=L rules as defensive-betting-analysis, but much stricter: default NOT_ELIGIBLE. Does not size stakes or manage money. In this workspace, also runs as section B of match-card-dual whenever the user pastes a match card with odds (alongside defensive PLAY). Use for 全權益滾注, 連勝, 5連勝, full roll, streak chain, streak-roll-eval, or dual card streak half.'
argument-hint: 'Same match line format as defensive-betting-analysis when screening fixtures. Optional: StreakState N/5 (current wins in chain). Output is streak eligibility, not ordinary PLAY size advice.'
user-invocable: true
---

# Streak-Roll Evaluation (Full-Equity Sequential Wins)

## What This Skill Does

This skill evaluates whether a **football Asian-handicap (or level-ball) spot** is fit to sit on a **full-equity sequential roll chain**: after each **Track B win**, the user may put **entire equity (principal + profit)** on the **next** single match, aiming for **5 consecutive wins** (default target; user may state another `k`).

**This skill does NOT:**

- change or replace `defensive-betting-analysis`
- set stake size, Kelly, or bankroll rules (user manages money)
- recommend parlays as a substitute for sequential roll
- rewrite historical `PASS` into retroactive streak legs

**This skill DOES:**

- inherit identity / external-odds / Track B settlement from the defensive framework
- **require fundamentals (基本面) as a mandatory modifier** — not optional flavor text
- apply **stricter** gates because **one Track B Loss ends the chain**
- output `STREAK_LEG` | `NOT_ELIGIBLE` | identity-hold, plus chain math notes

Football only.

## Relationship to defensive-betting-analysis

| Layer | Role |
|-------|------|
| `defensive-betting-analysis` | Ordinary WR-first single-leg / small-stake `PLAY` vs `PASS` |
| **`streak-roll-eval` (this skill)** | Whether a spot is fit for **full-roll sequential** use toward **5 wins** |
| `analysis-checklist.md` (this repo) | Ordinary pre-match upgrade/PASS execution — **stricter for streak** (see below) |
| `post-match-review-grok.md` | Settled ledgers for cross-check |

**Rule:** A spot that is ordinary `PLAY` under the defensive skill is **usually still `NOT_ELIGIBLE`** for streak-roll.  
Streak-roll requires **extra path robustness** beyond ordinary PLAY.

If the user only wants ordinary screening, use `defensive-betting-analysis`, not this skill.

### Dual card (this workspace)

When the user pastes a **schedule with odds**, the default orchestration skill is **`match-card-dual`**: always produce **defensive best PLAY/lean** **and** this skill’s **best STREAK_LEG or none**.  
You may be invoked alone for streak-only questions; on full odds cards, still pair with defensive unless user opts out.

## Primary Objective

**Maximize justified probability of completing a k-win sequential chain** under Track B settlement, default **`k = 5`**.

```text
Approx chain survival ≈ p₁ × p₂ × … × pₖ
(under independence assumption — state if legs are correlated)
Any Track B Loss → chain status BROKEN
```

Single-leg WR still uses:

- half-win = **W**
- half-loss = **L**
- push = **P** (excluded from WR; for streak chain see Push Policy)

**Full-equity roll implies:** path errors that are “acceptable Low PLAY” for unit bets are often **fatal** for the chain (example: favorite `-0.75` + draw = **L** — Houston lesson).

## Settlement Map (mandatory · same as defensive)

| Line (side bet) | Key outcome | Track B | Streak note |
|-----------------|-------------|---------|-------------|
| `-0.25` | draw | **W** | Friendlier to streak than `-0.75` |
| `+0.25` | draw | **L** | **Hostile** to streak |
| `-0.75` | win by 1 | **W** | Draw = **L** → **streak-hostile** |
| `+0.75` | lose by 1 | **L** | **Hostile** |
| `-1` | win by 1 | **P** | Chain pause / policy needed; not a clean W |
| `+1` | lose by 1 | **P** | Same |
| `-1.25` | win by 1 | **L** | Hostile |
| `+1.25` | lose by 1 | **W** | Only if structure supports dog buffer |

## Scope: What “Full-Equity Roll” Means Here

User-stated plan (evaluation context only):

1. After a Track B **W**, next stake may be **all current equity**.
2. Target: **5 consecutive Track B W** (unless user sets other `k`).
3. User handles actual staking; agent only grades **eligibility and path risk**.

## Hard Gates (block STREAK_LEG)

Same integrity as defensive skill, plus streak-specific:

1. Match **identity** uncertain → ask user / no STREAK_LEG  
2. Wrong-match external odds → invalidate  
3. No usable market / unreadable line → NOT_ELIGIBLE  
4. Incomplete external 1X2 with no trustworthy lean → NOT_ELIGIBLE  
5. True coin-flip / no external lean → NOT_ELIGIBLE  
6. Any **single score path that is common and = L** without a clear structural reason it is rare → prefer NOT_ELIGIBLE  

## Streak-Hostile Structures (default NOT_ELIGIBLE)

Mark **NOT_ELIGIBLE** when **any** apply (workspace lessons folded in):

| Structure | Why fatal for full-roll |
|-----------|-------------------------|
| Favorite **`-0.75`** (unless user explicitly accepts draw=L and leg is rare exception — default **no**) | Draw = full chain death (Houston) |
| Favorite pure **`-1`** | Win-by-1 = push, not W; messy for roll |
| Deep fav **`-1.5` / `-2` / deeper** | Small win kills (Fener / Başakşehir class) |
| Dog **`+0.25` / `+0.75`** thin | Half-loss paths; Rochdale class |
| Light external lean + shallow fav (esp. `-0.25` with home ~≥1.85) | León / light-kill class |
| NPL / Aus cup / USL / semi-pro | High variance (Knights) |
| Euro / cup **first leg** medium deep | Aggregate ≠ single-match; noise |
| Friendship / CLB / preseason | Extreme variance |
| Near-even / incomplete screen | No edge |
| Same-night second leg same branch as prior full-roll leg | Correlation / variance stack |

## Streak-Friendlier Structures (still rare)

Only candidates for further checks (not auto-yes):

| Structure | Why better for chain |
|-----------|----------------------|
| League fav **`-0.25`** + **clear** external lean (roughly home/away win **≤1.70**, not light) | Draw = **W** under Track B |
| Level ball + **clear** external lean (牙山 class; lean not tiny) | Draw = push (not L); win = W |
| League fav **`+1` dog buffer** only if external dog lean is structural and price not thin garbage | Lose-by-1 = P (not L) — still high bar |

**Even these** need identity lock, named sources, clean fundamentals, and non-hostile branch history.

## Fundamentals (基本面) — mandatory for streak

Odds alone are **never** enough for `STREAK_LEG`. Full-equity chains die on **script risk** (rotation, dead rubber, travel, key absences) even when 1X2 looks Medium/Strong.

### Minimum fundamentals snapshot (write explicitly)

For every candidate, state what is known / unknown:

| Factor | Ask / note |
|--------|------------|
| Season phase | mid-season vs end / break / restart |
| Motivation | table fight, cup priority, dead rubber, rest rotation |
| Personnel | striker / GK / playmaker absences; two+ spine out → usually **NOT_ELIGIBLE** |
| Home/away | real home edge vs neutral / hostile travel |
| Schedule | congestion, 3-in-7, long travel, time zones |
| Style / game state | low-block draw magnet vs open favorite |
| Competition noise | cup leg, two-leg aggregate, youth/B side |

### Fundamentals vetoes (default NOT_ELIGIBLE even if odds look good)

- Clear **rotation / B-team / rest** risk for the side you need to cover  
- **Dead rubber** or inverted motivation (favorite has nothing to play for)  
- **Structural absences** that change how the side creates/prevents goals  
- **Severe travel / congestion** on the favored side for an aggressive line  
- Unknowns dominate (no form/news readable) → treat as incomplete → **NOT_ELIGIBLE** for streak (ordinary defensive may still PASS/lean only)

### Fundamentals support (raises confidence only after odds+path clear)

- Consistent XI / no red-flag absences  
- Motivation aligned with market lean  
- Home in stable league mid-season  
- Style not draw-heavy when line is killed by draws (esp. if considering any borderline line)

**Modifier rule (same hierarchy spirit as defensive skill):**  
Fundamentals **may block or downgrade** a streak leg; they **must not invent** a STREAK_LEG against external lean + Track B path.

## Streak Eligibility Gate (all must pass for STREAK_LEG)

1. [ ] Identity locked (teams, competition, home/away, leg)  
2. [ ] External source **named**; lean **clear** (not light, not near-even)  
3. [ ] Competition: **stable domestic league** preferred; cup/qual **first leg** almost never  
4. [ ] Line is **not** in Streak-Hostile table (default)  
5. [ ] Track B map written: every relevant score → **W / L / P**; **state which L paths kill the chain**  
6. [ ] Preferred: **no common L path** (draw is W or P, not L)  
7. [ ] Self cross-check vs `post-match-review-grok.md` — branch not kill-heavy; **low-similarity history must not drive STREAK_LEG**  
8. [ ] **Fundamentals snapshot written**; **no veto** from table above; unknowns disclosed  
9. [ ] Fundamentals **align with** lean (do not fight market without extraordinary evidence — and even then usually NOT_ELIGIBLE for streak)  
10. [ ] Chain slot stated: `StreakState m/k` (e.g. `0/5` start, `2/5` after two W)  
11. [ ] Honest: sample size small → confidence **Low** even if STREAK_LEG  

If any fail → **`NOT_ELIGIBLE`**.

## Confidence Cap

- Streak-roll **never** outputs Confidence **High**  
- Max **Medium** only if external Strong gap + clean league `-0.25` (draw=W) + clean history  
- Default STREAK_LEG confidence: **Low**  
- Ordinary defensive PLAY samples (Houston, Knights, light kills) **raise** the bar, not lower it  

## Push Policy (full-roll chain)

Track B push is **not** a Win.

For chain reporting:

```text
Push → Chain status: PAUSE (equity returned without progress toward k)
Do not count push as a win toward 5.
Do not invent a “push = continue as win” rule.
User decides whether to re-stake same equity on a later leg after pause.
```

## Chain State Machine

```text
IDLE / BROKEN → only start with STREAK_LEG on a new chain (0/5)
ON_STREAK (m/k), m < k → next leg must again be STREAK_LEG; else advise PAUSE_CHAIN (do not force a weak leg)
m = k → TARGET_HIT (5/5 default); stop or user restarts
Any Track B L → BROKEN immediately
```

When screening a **batch**, do **not** pick five legs at once as if independent parlays.  
Evaluate **one next leg** at a time (or rank candidates for the **single next** slot).

## Batch Mode

If user pastes many lines:

1. Run identity + external + Track B path for each  
2. Sort into: `STREAK_LEG candidates` (rare) / `NOT_ELIGIBLE`  
3. At most **one** recommended next streak leg (or zero)  
4. Do not build a 5-leg “package” that implies simultaneous risk  

## Self Cross-Check (streak)

Same procedure as defensive skill, plus:

- Prefer ledger branches with **draw-friendly** paths for streak  
- Cite Houston (`-0.75` draw=L), Fener (deep small-win kill), Knights (NPL), light `-0.25` kills as **anti-examples**  
- Cover-after-PASS or lean-side hits **must not** create STREAK_LEG  

## Output Format

```markdown
Streak Decision: STREAK_LEG | NOT_ELIGIBLE | IDENTITY_HOLD
Chain Target: 5 consecutive Track B wins (or user k)
StreakState: m/k (e.g. 0/5)
Market: [line or none]
Primary lens: Streak survival (Track B sequential) + WR path + **fundamentals**
WR path map: [score → W/L/P; highlight chain-kill L paths]
Draw impact: W | L | P  (must be explicit)
External gap: Strong | Medium | Light | None
Fundamentals:
- Motivation / season phase: ...
- Personnel: ...
- Home-away / travel / schedule: ...
- Style / draw risk: ...
- Veto? Yes/No + why
Source Basis: ...
Sources Used: ...
Why streak-eligible / why not: [odds path + fundamentals gates]
Self Cross-Check:
- Branch: ...
- Comparables: ...
- Similarity: ...
- Anti-examples applied: [e.g. Houston draw=L on -0.75]
Approx chain note: if this leg p̂ ≈ x, five i.i.d. legs ≈ x^5 (illustrative only; n small)
Confidence: Low | Medium
Rejected for streak: [ordinary PLAY candidates that fail path or fundamentals]
Risk Notes: [scoreline kills + script kills]
Fund management: user-owned — no stake size prescribed
```

### Post-match (streak chain)

```markdown
Chain before: m/k
Leg market: ...
Score: ...
Track B: W | L | P
Chain after: m+1/k | BROKEN | PAUSE (push)
Assessment: Path held | Kill path hit | Eligibility misgrade
Sample Status: Observation only unless repeated eligibility misgrades
```

## Decision Priorities (streak)

0. Identity / wrong-match / no market (hard)  
1. **Chain-kill path risk** (common L outcomes under Track B, esp. draw on `-0.75`)  
2. External clear lean + stable league structure  
3. **Fundamentals veto / alignment** (rotation, motivation, personnel, travel)  
4. Branch history (hostile branches → NOT_ELIGIBLE)  
5. Ordinary defensive PLAY quality — **necessary but not sufficient**  
6. Price / capital aesthetics — irrelevant to eligibility  

## Quality Checks

Before finishing:

- [ ] Did **not** edit `defensive-betting-analysis` rules  
- [ ] Did **not** prescribe all-in amount or “how much to stake”  
- [ ] Default bias is **NOT_ELIGIBLE**  
- [ ] `-0.75` draw=L stated if that line considered  
- [ ] **Fundamentals block present** (not odds-only)  
- [ ] Push not counted as streak win  
- [ ] Named sources for any external claim  
- [ ] Identity locked or user asked  
- [ ] Ordinary `PLAY` from defensive skill **not** auto-mapped to STREAK_LEG  
- [ ] Workspace ledgers used for anti-examples when relevant  

## Response Style

- Lead with **Streak Decision**, **draw impact**, and **fundamentals veto Y/N**  
- Be explicit that **one L zeros the roll plan’s progress**  
- Prefer fewer STREAK_LEG calls over filling a 5-leg fantasy card  
- If nothing qualifies: say so plainly — empty candidate list is a valid success of the filter  

## When to Use

Use when the user wants:

- full-equity / all bankroll sequential roll evaluation  
- 連勝 / 5連勝 / 全權益滾注 / streak chain fitness  
- whether a match can be **next leg** after a win  
- contrast between ordinary PLAY and streak-eligible legs  

Do **not** use this skill for ordinary single-unit defensive screening alone — use `defensive-betting-analysis` instead.
