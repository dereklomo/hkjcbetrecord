---
name: defensive-betting-analysis
description: 'Analyze football betting with win-rate (贏盤率) as the primary objective under half-win=win and half-loss=loss settlement; external odds, branch history, and fundamentals support the WR case while capital/price/cup filters are auxiliary. HKJC prices are usually execution overlay. In this workspace, when the user pastes a match card with odds, default to match-card-dual: report best defensive PLAY/lean and best streak-roll STREAK_LEG (if any). Read analysis-checklist.md first, then post-match-review-grok.md ledgers. Football only. Use for 足球, 讓球, 受讓, odds paste, pre-match, 賽程賠率, PASS vs PLAY, or post-match review.'
argument-hint: 'Preferred HKJC card paste (multi-line): FBxxxx · 聯賽 · 開賽時間 · 主隊 [主盤] ： 客隊 [客盤] · 主讓球水 · 客讓球水. Also accept one-line: 勝A 隊A 讓A 盤 讓B 隊B 勝B. Unlabeled = HKJC execution; seek external benchmarks for direction. Use *盤 for synthetic 3-way→AH.'
user-invocable: true
---

# Defensive Betting Analysis

## What This Skill Does

This skill turns a compact betting ruleset into a readable decision workflow for football match analysis.

This skill is for football only. Do not apply it to basketball, baseball, tennis, esports, or other sports.

**Primary goal: maximize justified handicap win-rate (贏盤率)** under half-win = win and half-loss = loss. All other factors are auxiliary.

Build the market view from external prices first, preferably Pinnacle, other sharp or market-making books, and then bet365 or similar major references — as the main **base-rate** input to win-rate. Use Hong Kong Jockey Club mainly as the user-supplied **execution** price relative to that external benchmark.

Whenever the user provides only HKJC or unlabeled prices, the analysis should actively seek accessible external public odds or public odds summaries before forming a directional view. If such a public source is used, the final write-up must explicitly name it.

If the user provides only external odds, do not force the analysis back into a Hong Kong Jockey Club-first framing.

For this workspace and user workflow, if odds are pasted without a source label, treat them as HKJC prices rather than external odds. The default next step is to look for external benchmark prices and use those benchmarks, not the HKJC line itself, to determine the preferred betting direction.

Do not claim external confirmation unless a concrete public source was actually checked. Never imply unseen Pinnacle, bet365, or consensus support.

### Workspace read order (this repo)

For **pre-match** screens in this workspace, read files in this order (does **not** change hard rules below):

1. **`match-card-dual` skill** (`.github/skills/match-card-dual/SKILL.md`) — **default orchestration** when user pastes schedule + odds  
2. **`analysis-checklist.md`** — execution checklist, PLAY upgrade / PASS gates, watchlist (execution layer only; if it conflicts with this skill, **this skill wins**)
3. **`post-match-review-grok.md`** — **hot** ledgers only (table A / A′ / branch WR + mistake short table). Do **not** load full archive by default.  
4. **`post-match/INDEX.md`** → open only the **latest 2** files under `post-match/batches/` for recent §9 / narrative. Open `batches/2026-07-archive-legacy.md` **only** for formal review or missing comparables.  
5. **`record/pre-match/INDEX.md`** / dated pre-match cards — same-day prior decisions when present  
6. **`record/hkjc-name-alias.md`** — HKJC Chinese↔English club map (**read before external search**; append only after user confirms)  
7. **`streak-roll-eval` skill** — for streak section of dual card (stricter; separate from ordinary PLAY)

Do **not** treat `post-match-review-home.md` as a writable review source (redirect/stub only).  
Do **not** invent a second checklist or re-home watchlists into ad-hoc session notes.

### Dual card default (this workspace)

When the user pastes **one or more fixtures with odds** (HKJC-style lines, multi-match card, dated 賽程+賠率):

1. Always run **this skill** with **`analysis-checklist.md`**:  
   - **Table A formal PLAY** = ideal gates + **whitelist W1–W6** only (**0 / 1 / many** legs; **no** §1-max)  
   - **準表 A′ (Soft PLAY)** only if table A empty — 0–1 leg, small stake OK, post-match **table A′** only (never merge into table A WR); pure Obs column **retired**  
2. Run **`streak-roll-eval` only if** table A has a **draw-friendly** formal (`-0.25` or level-ball lean); else `streak: skipped` unless user forces streak.  
3. Use the response skeleton in `match-card-dual` (A′ section always present).  
4. Skip dual only if user says only-defensive / only-streak, or input is post-match-results-only.  
5. **Post-match context:** prefer **hot** `post-match-review-grok.md` + **2** recent batches from `post-match/INDEX.md` — not the full legacy archive.

Ordinary hard rules below are unchanged. **Primary workspace goal:** table-A Track B win-rate (grill-locked); **A′** is the frequency valve, not a table-A relax.

## Primary Objective: Win-Rate First (贏盤率為主)

**Hierarchy (mandatory):**

1. **Primary — Handicap win-rate (贏盤率 / Track B)**  
   Choose and evaluate sides/lines by estimated probability of **settling as a win** under the win-rate ledger (half-win = win, half-loss = loss, push excluded).  
   Directional recommendation and line selection are driven mainly by: external market lean → comparable branch WR → scoreline path needed to win the line.

2. **Auxiliary — All other information**  
   Use only to **support, adjust confidence, or veto** the win-rate read — never as the main reason to ignore a clearly superior win-rate line:
   - capital / payout / “stable-profit” narrative
   - thin price aesthetics
   - cup reject zones, anti-low-value, blacklists (except hard identity / wrong-match / no-market)
   - fundamentals (基本面), motivation, travel, rotation
   - HKJC overlay quality

**Default posture:** prefer the line/side with the **best justified win-rate** after external odds + branch history + fundamentals as *modifiers*.  
`PASS` when no side has a usable win-rate edge (true coin-flip, identity broken, or insufficient data) — not merely because price is “thin.”

Still intentionally careful:
- Identity must be correct before any odds attach.
- Do not invent external prices.
- Do not treat payout maximization as primary (payout is auxiliary).
- Fragile or contradictory win-rate evidence → lower confidence or `PASS`.

## Dual Track (Primary = B, Auxiliary = A)

### Track B — Win-rate ledger (**PRIMARY**)

- Purpose: measure and forecast **handicap settlement hit rate**; drive line choice and `PLAY` lean.
- Ignores odds magnitude and staking returns for the score itself.
- Settlement convention (mandatory):
  - **half-win (贏半) = Win**
  - **half-loss (輸半) = Loss**
  - **full win / full loss** as usual
  - **push / void (走盤)** = exclude from win-rate denominator (report push rate separately; never count as Win)

Win-rate formula:

```text
WR = Wins / (Wins + Losses)
Pushes excluded from denominator
```

### Half-line quick map (Track B)

| Line (side bet) | Key outcome | Track B |
|-----------------|-------------|---------|
| `-0.25` | draw | **Win** (half-win) |
| `+0.25` | draw | **Loss** (half-loss) |
| `-0.75` | win by 1 | **Win** (half-win) |
| `+0.75` | lose by 1 | **Loss** (half-loss) |
| `-1` | win by 1 | Push (exclude) |
| `+1` | lose by 1 | Push (exclude) |
| `-1.25` | win by 1 | **Loss** (half-loss) |
| `+1.25` | lose by 1 | **Win** (half-win) |

Implication: Track B is **more favorable to favorite `-0.75`** (win-by-1 counts Win) and **harsher on underdog `+0.75`** (lose-by-1 counts Loss) than cash P&L under half-settlement.

### Track A — Capital / filter layer (**AUXILIARY**)

- Purpose: optional risk notes, price quality, and hard safety gates — **not** the main selection engine.
- May **downgrade confidence**, prefer a different line with similar WR, or **veto** only when a **hard gate** fires (below).
- Must **not** reject a clear win-rate-preferred line solely because “stable-profit / thin price / cup habit PASS.”

**Hard gates still block PLAY (identity & data integrity only):**
- match identity uncertain → ask user / no PLAY
- wrong-match external odds → invalidate screen
- no usable market identity or unreadable line → PASS

**Soft gates (auxiliary only):** cup reject zone, anti-low-value, coin-flip / no-lean PASS, logistics, motivation, thin price — use to **rank, warn, or modestly reduce stake confidence**, not to auto-kill a line that win-rate analysis prefers unless the win-rate edge itself is weak/absent.

**True coin-flip is defined from external pricing first, not from equal executable AH prices alone.** Symmetric HKJC/AH quotes (e.g. both sides 1.88) without a checked external 1X2/AH lean are incomplete screens, not automatic hard blacklists. If external shows a usable lean, apply Level-Ball / PK External-Lean Overlay (or the corresponding shallow-line WR path) instead of forcing PASS by habit.

### Building a win-rate-first recommendation (pre-match)

1. **Identity lock** (hard).
2. **External odds** → which side is stronger and how strong (primary market signal for base rates).
3. **Map to line(s)** under Track B settlement (what score paths are W vs L).
4. **Branch WR + self cross-check** from settled reviews (comparable spots only).
5. **Fundamentals as modifier** — raise/lower WR confidence; do not flip side without market+WR support.
6. **HKJC overlay** — execution price; auxiliary.
7. **Track A soft filters** — risk notes; only veto if WR edge is not actually clear.
8. **Workspace execution gate** (this repo): apply `analysis-checklist.md` for formal PLAY upgrade vs PASS / WR-lean-only (light `-0.25` force PASS; `-0.75` draw=L + **draw-magnet default no formal**; **named external required**; dog `+1` formal only if fav external **>1.50**; league tier A/B/C/D; **§1d 防漏升**). Checklist cannot override hard identity / wrong-match / no-market gates in this skill.
9. Output: preferred line for **win-rate**, then auxiliary caveats.

### Using Track B for unplayed matches (line push / inference)

1. **External odds first** — 1X2 / AH lean and implied strength gap.
2. **Settled WR ledger** — comparable branches only.
3. **Fundamentals** — auxiliary modifier (not a substitute for market+WR).
4. State `WR-inferred lean` with confidence; this **is** the main predictive output.
5. Soft capital filters may annotate risk; they do not redefine the primary objective away from win-rate.

### Half-lines under win-rate-first

- `±0.75` is **not** an automatic blacklist.
- Favorite `-0.75` is often **more attractive on Track B** (win-by-1 = W) than old stable-profit habit suggested — evaluate on WR, not “thin cup PASS by default.”
- Underdog `+0.75` is **harsher on Track B** (lose-by-1 = L); prefer deeper buffers (`+1`) when WR paths are similar.

## Self Cross-Check Against Similar Settled Spots

Before finalizing Track A `PLAY`/`PASS` (and when writing Track B `WR-inferred lean`), **run a self cross-check**: compare the current fixture to **comparable settled samples** in this workspace, then adjust confidence or decision only when the comparison is structurally valid.

Primary local sources (in order):
1. `analysis-checklist.md` — pre-match execution checklist, upgrade/PASS gates, and watchlist (execution layer; does not override this skill’s hard rules)
2. `post-match-review-grok.md` (and any dated post-match review files / `record/pre-match-*.md`) — settled ledgers table A/B and batch reviews
3. Branch tags / Track B ledger tables already written in those reviews
4. Optional: `post-match-review-gpt.md` or other agent reviews for disagreement notes — never copy a decision without re-checking identity and external odds

### When self cross-check is required

Run it when **any** of these apply:
- multi-match batch screening
- borderline Track A call (could be PLAY or PASS)
- Track B line-push / unplayed inference is stated
- level-ball, `±0.75`, Australian semi-pro/NPL, cup favorite medium-deep, or second-leg spots
- user asks to cross-check, re-analyze, or compare to history
- prior agent (e.g. GPT) disagreed with a similar spot

Skip only for trivial identity-hold questions with no market decision.

### Self cross-check procedure (fixed order)

1. **Lock the current spot**
   - identity (teams, competition, leg)
   - external lean and **strength tier** of lean (for cup `-0.75`/`-1`: strong / medium / light gap — see Cup Favorite zone)
   - quoted line(s) under consideration
   - fundamentals snapshot (form, motivation, personnel, venue, travel, rotation, cup state)

2. **Assign a branch tag** (examples — reuse existing tags when possible)
   - `cup-fav-0.75` / `cup-fav-1.0`
   - `league-fav-0.75` / `league-shallow-0.25`
   - `dog-plus0.75` / `dog-plus1` / `aus-semi-dog-shallow`
   - `level-ball-lean` vs `near-even` / `true-coin-flip`
   - `second-leg` / `npl-aus` / `youth-or-B-team`
   - Do **not** merge tags that only share the same number (e.g. all `-0.75`) if structure differs (cup vs league, strong gap vs bottom-table derby).

3. **Pull comparable settled cases**
   - Prefer same branch tag + similar external price band + similar competition tier.
   - For each case note: original Track A decision, score, Track B W/L/P, and **why it is or is not comparable**.
   - State sample size honestly (`n=0` / `n=1` / small / mixed).

4. **Score similarity (qualitative is enough)**
   - High: same branch, similar 1X2 band, similar competition noise, similar motivation/leg state.
   - Medium: same line family but different tier or motivation.
   - Low: only the handicap number matches (e.g. NPL `-0.75` vs Eliteserien blowout `-0.75`).
   - **Low-similarity history must not drive PLAY.**

5. **Apply outcomes correctly (no hindsight abuse)**
   - Historical Track B **W** on a line the skill **PASS**ed does **not** mean current should be PLAY.
   - Historical Track B **L** supports caution on that branch but does not alone invent new Force-PASS rules unless the adjustment gate is met.
   - Distinguish: kill-line validation (PASS avoided a losing deep favorite) vs cover-after-PASS observation (favorite covered; still may stay PASS).
   - Level-ball: only cite `level-ball-lean` samples (e.g. external away lean + PK) when current external lean exists; never cite them for true near-even PK.

6. **Reconcile to dual track (WR primary)**
   - Track B: cross-check is the main input to `WR-inferred lean` and line choice (`Low` if n small, noise high, or similarity low).
   - Auxiliary filters: may lower confidence or note capital risk; do not override a clear WR-preferred line without showing the WR case failed.
   - If history is mixed (branch has both W and L), state mixed WR and use external + fundamentals to break ties — or `PASS` if no edge remains.

7. **Write a short cross-check block in the analysis** (required when step was run)

```markdown
Self Cross-Check:
Branch: [tag]
External gap tier (if cup-fav-0.75/1.0): Strong | Medium | Light | n/a
Comparables: [2–5 named settled spots + W/L/P or PASS outcome]
Similarity: High | Medium | Low
Implication (WR primary): WR-inferred lean / none; confidence Low|Medium
Implication (aux filters): price/cup notes only; no thin-price-only veto if WR clear
```

### Hard anti-patterns (self cross-check)

- Do not treat “same Chinese handicap wording” as same branch.
- Do not use cup-fav cover rates to justify NPL bottom-table `-0.75` PLAY.
- Do not use Australian `+1` hits to justify thin `+0.25`/`+0.75` PLAY without price and structure.
- Do not use 牙山/Vancouver-style PK samples when current external 1X2 is symmetric.
- Do not rewrite historical PASS into retroactive PLAY while cross-checking.
- Do not skip fundamentals because a branch WR looks high.
- Empty NPL/aus-semi ledger → label inference **Low** and keep Track A conservative.

### Canonical branch reminders (workspace memory)

These are patterns already stressed in local reviews — use as lookup keys, not as auto-bets:

| Branch idea | WR-first note (primary) | Auxiliary note |
|-------------|-------------------------|----------------|
| Cup favorite `-0.75` to `-1` | Mixed W/L; tier external gap (strong/medium/light); `-1` needs stronger tier than `-0.75` | Cup noise lowers confidence; no auto-reject / no auto-PLAY |
| League strong favorite `-0.75` | Often solid WR path (win-by-1 = W) | Thin price is secondary |
| Shallow league `-0.25` | Draw = W for favorite — often high WR; **do not veto solely for thin price** | Thin price only downgrades confidence |
| Level ball + clear external lean | Prefer lean side on 0 for WR | Needs lean; not true coin-flip |
| True / near coin-flip PK | No WR edge → PASS | — |
| Aus semi-pro / NPL dog shallow | High variance; small n | Prefer deeper buffer (`+1`) for WR paths when possible |
| Aus semi-pro dog `+1` | Lose-by-1 = push (exclude); often better WR path than `+0.25` | — |
| Second leg / aggregate | Single-match WR ≠ tie outcome | Aggregate state is a strong WR modifier |

## When to Use

Use this skill when the user wants to:
- evaluate a football match before kickoff
- decide whether to bet or pass
- screen a favorite vs underdog handicap market
- choose between a single bet and a parlay leg
- apply a defensive betting framework
- avoid low-value favorites and noisy markets
- review a finished match and decide whether the strategy needs adjustment
- self cross-check a screen against past similar settled handicaps or branch WR
- re-analyze after dual-track or historical comparison

**In this workspace (pre-match):** on **odds-card paste**, follow **`match-card-dual`** (defensive + streak-roll both). Read **`analysis-checklist.md`**, then **`post-match-review-grok.md`** ledgers. Append new settled results only to `post-match-review-grok.md` (or dated review/pre-match record files)—not to `post-match-review-home.md`.

**Do not archive user input (2026-08):** never dump the user’s raw paste into `selfnote.txt` or pre-match「原始盤」unless they **explicitly** ask to write/save a file. Default = decision output only.

## Market Source Policy

Use these source layers:
- primary analysis source: external bookmaker odds, with preferred benchmark order of Pinnacle, other sharp or market-making books, then bet365, then other major books
- secondary reference source: Hong Kong Jockey Club prices for relative comparison, local-bias checking, and optional execution mapping

Apply these scope rules:
- for this workspace, if the source is not stated, assume the user is giving **HKJC execution** prices rather than external prices
- when the user gives only HKJC prices, actively seek an accessible external benchmark for **direction / gap tier** (not to match HKJC waters)
- if the user provides external odds, treat them as the main **directional** signal unless explicitly told otherwise
- if the user provides multiple external sources, prioritize sharp or consensus pricing over isolated soft-book prices
- **Identity / source-error gate (Grill 2026-08 · hardened):** see **Match Identity Gate** below — **ask user** when English names or fixture identity do not lock; **do not guess**
- **Never require external odds to equal HKJC waters.** Mismatch of decimal prices is **not** “identity fail.” HKJC = execution; external = identity check + lean/strength only
- use HKJC only after identity is locked and external direction (if any) is formed; HKJC must not be the core reason for a bet by itself
- if the user provides only non-HKJC prices, continue normally and label the result as external-market analysis
- if the user provides only HKJC prices and an external benchmark is found, label the result as HKJC input plus external benchmark analysis
- if the user provides only HKJC prices and no external benchmark can be obtained, analysis is still allowed, but clearly mark incomplete value screening
- when any external public benchmark is used, identify the **exact source + concrete 1X2 or AH numbers** (traceable); ban vague “market leans home” with no figures
- **`Sources Used: none` is valid** when nothing was checked; never invent Sources
- if two external sources **conflict on direction**, treat as **no clean lean** (incomplete) — list both; do not pick a side to force Medium/PLAY
- never attach external 1X2/AH from a different match, club, or competition to an HKJC line; wrong-match odds **invalidate that fixture’s external screen**

## Efficient Public Source Guidance

To reduce token waste, prefer sources that have already produced usable public data in this workspace before exploring many new sites.

Successful source order for public match information:
- `Bing` search-result summaries for quick 1X2 odds snippets, matchup identity checks, and visible source links
- `Sporting Life` for readable market odds and match context when available
- `BetMines` for 1X2 odds, implied probabilities, and basic form or rhythm context when extraction works
- `Legalbet` and similar summary pages when Bing exposes the matchup and odds cleanly
- `Bettors.Club` only as a partial source when bookmaker rows or fragments are visible; do not assume full AH coverage from it
- `Tipsme`, `500.com`, `NowScore`, `Titan007`, or similar public odds pages when they expose concrete bookmaker rows or readable odds snapshots

Source-efficiency rules:
- start with `Bing` summary extraction before attempting many direct site fetches
- prefer sources that expose concrete 1X2 numbers in plain text over JS-heavy odds pages
- if `Sporting Life`, `BetMines`, or a Bing summary already gives a usable external direction, stop broad searching and move to analysis
- do not spend excessive tokens chasing exact Asian handicap tables when only 1X2 is realistically accessible; label the result as partial or incomplete instead
- if a source repeatedly fails meaningful extraction in this workspace, deprioritize it during the same analysis session
- every final recommendation that uses an external view must include `Sources Used` with **named pages + concrete odds figures** actually checked (not vague lean language)
- `Sources Used: none` when no external page was checked — preferred over fabricated sources

Known limitations in this workspace:
- direct bookmaker or comparison pages are often JS-heavy, rate-limited, or blocked
- `DuckDuckGo` may return anti-bot or challenge pages
- public odds access is usually strongest for `1X2`, weaker for exact sharp `AH` tables
- when no robust external benchmark is retrievable, explicitly downgrade the result instead of simulating certainty

## Required Inputs

Collect or infer these inputs before making a recommendation:
- favorite team and underdog team
- current season phase for both sides
- current round or whether the match is in the last three league rounds
- motivation level for both sides
- important team news, especially structural absences such as the main striker, first-choice goalkeeper, or primary playmaker
- whether the home side has a meaningful normal home-ground advantage and whether the away side is materially weaker away from home
- travel burden and schedule density, especially long-distance away trips, time-zone shifts, or three matches in seven days
- whether the tie is a two-legged knockout, and if so whether the match is the first leg or second leg
- market type being considered
- external benchmark direct win odds, if available
- external benchmark handicap line
- whether this is a single bet or part of a parlay
- optional multi-book line comparisons
- Hong Kong Jockey Club comparison prices, which in this workspace are often the main user-provided input

If key structural inputs are missing or uncertain, default to `PASS` or clearly mark the analysis as incomplete.

**Exception for identity:** if match identity is uncertain, do not silently finish with a guessed `PASS`/`PLAY`. Ask the user first (see Match Identity Gate). Only after identity is confirmed may analysis continue.

If external benchmark direct win odds are missing, do not invent them and do not silently replace them with HKJC alone. Continue only if the handicap-based rules are still usable, and explicitly state that the `Anti-Low-Value Rule` was not fully checked and that the result is only a partial value screen.

If external benchmark prices are referenced, they must be tied to named public sources actually checked during the analysis, and those sources must refer to the same verified teams and fixture.

## Preferred Match Input Format

**Workspace default (2026-08 起 · 最新權威)：港盤多行卡（HKJC card block）。**  
One-line compact format remains **accepted fallback** (English dumps, quick paste).  
Unlabeled prices = **HKJC execution**; seek external benchmarks for direction (unchanged).

### Format A — HKJC card block（**preferred / 最新**）

Typical paste (tabs or spaces; fields may wrap across lines):

```text
FBxxxx        聯賽中文名    HH:MM    主隊中文名
[ 主盤 ]    ：    客隊中文名
[ 客盤 ]        主讓球水            客讓球水
```

Optional prefix lines (when present, parse them):

```text
DD/MM/YYYY HH:MM
FBxxxx
English Competition Name
Home English
Away English
[line] or media tags (MTS / YT / …)
```

**Field map:**

| 欄位 | 含義 | 備註 |
|------|------|------|
| `FBxxxx` | 港會球賽編號 | 鎖定身份／對帳用；寫 pre-match 可帶編號 |
| 聯賽 | 中文（或英）賽事名 | 阿甲／美職／瑞士超／女非國盃… → 分檔 A/B/C/D |
| `HH:MM` | 開賽時間 | 通常為**用戶本地／港盤列表時間**；跨日卡以用戶標的賽事日為準 |
| 主隊 / 客隊 | 中文譯名為主 | 對英名見 Match Identity Gate；勿瞎猜 |
| `[ 主盤 ]` | 主隊讓球（港式） | 可含空格：`[ +1 / +1.5 ]`、`[ 0 ]`、`[ -2 ]` |
| `：` | 對陣分隔 | 可有可無 |
| `[ 客盤 ]` | 客隊讓球 | **應與主盤符號相反、數值對應**（主 -0.5/-1 ↔ 客 +0.5/+1） |
| 主讓球水 / 客讓球水 | 兩側 **讓球盤水位** | 本格式常**不帶** 1X2 獨贏水 → 標 incomplete 獨贏過濾，仍可篩盤 |

**Worked example (user 2026-08-02 style):**

```text
FB2115        阿甲    04:58    學生隊
[ -0.5 / -1 ]    ：    防衛者
[ +0.5 / +1 ]        1.78            2.04
```

→ Home **Estudiantes (LP)** (verify identity) **-0.5/-1 @1.78** vs Defensa **+0.5/+1 @2.04**；competition 阿甲；kickoff 04:58 on card date.

```text
FB2075        葡萄牙超級盃    03:15    波圖
[ -2 ]    ：    杜連斯
[ +2 ]        1.86            1.90
```

→ Porto **-2 @1.86** vs Torreense **+2 @1.90**；深讓；`Strong 獨贏 ≠ 深讓 WR` 紀律適用。

**Parsing rules for Format A:**

1. **One fixture = one FB block** until the next `FBxxxx` (or next clear competition+time row).  
2. **Home is listed first** (left of `：`); handicap in the first `[…]` is **home’s** line.  
3. **Normalize lines:** strip spaces inside brackets; `/` half-lines → Track B map (`-0.5/-1` = **-0.75**, `0/-0.5` = **-0.25**, `+1/+1.5` = **+1.25**, plain `[0]` = level ball).  
4. **Home negative = home gives goals** (favorite side on AH); home positive = home receives (dog).  
5. **Odds order:** first water = **home AH**, second = **away AH** (not 1X2 unless user labels 獨贏).  
6. **Missing 1X2 is normal** in this paste → do **not** invent ML; seek external 1X2; mark Anti-Low-Value / full value screen incomplete when needed.  
7. **Chinese names:** high-ambiguity → **ask user** (學生隊 vs 里奧古亞圖學生隊；國際… 等).  
8. **Media tags** (`MTS`, `YT`, …) → ignore for market math; optional note only.  
9. **Line moves vs prior card:** if user re-pastes same day with new waters/lines, treat as **latest execution**; re-screen or note delta (e.g. Miami was -0.75, now pure -1).  
10. **Multi-card day:** several pastes same date → merge under one 賽事日 file when user asks 合併; each paste can be card a/b/c in analysis.

### Format B — One-line compact（**fallback · 仍接受**）

```text
隊伍A勝賠率 隊伍A 隊伍A讓球賠率 讓球盤 隊伍B讓球賠率 隊伍B 隊伍B勝賠率
```

Example:

```text
1.62 Liverpool 1.86 -1.25 2.02 Burnley 5.20
```

Synthetic 3-way→AH: leading `*` on the line (`*-1.5`).

Shorthand (incomplete — no ML):

```text
隊伍A 隊伍A讓球賠率 讓球盤 隊伍B讓球賠率 隊伍B
```

Interpret Format B as:
- `隊伍A勝賠率` / `隊伍B勝賠率`: match-winner odds when present  
- `讓球盤`: always from side A (first team) perspective  
- negative = side A gives goals  

Also accept English one-line dumps of the form  
`ML_home Home AH_home [line] AH_away Away ML_away`  
(as historically used in this workspace).

### Shared parsing rules (A + B)

- treat handicap market as default unless user states O/U only  
- unlabeled → **HKJC prices** (execution overlay)  
- external primary only when user labels external or provides cross-book context  
- **if team / competition / leg identity unclear → ask before PLAY or external attach**  
- do not resolve ambiguous HKJC Chinese by first web hit (e.g. `國際杜古` ≠ Inter Escaldes)  
- when HKJC-only, seek external benchmark before directional recommendation  
- if handicap begins with `*`, synthetic from 3-way; lower confidence  
- if user supplies external + HKJC, direction from external; HKJC overlay only  
- if only HKJC and no external, incomplete value screen — say so  
- Format A without ML: mark direct-win filter missing; still allow dual card on AH paths  
- Format B shorthand without win odds: incomplete screening, not fully cleared PLAY  
- if line cannot be parsed cleanly, stop and request correction instead of guessing

## Post-Match Review Workflow

When the user provides a result after the match, use it for strategy review rather than for outcome chasing.

Review in this order:
1. Restate the original decision: `PASS` or `PLAY` (Track A)
2. Record the actual market outcome against the original quoted line (cash / standard AH if needed)
3. **Log Track B win-rate settlement** for the primary quoted handicap and any key alternative line: `W` / `L` / `P` under half-win=Win, half-loss=Loss
4. Tag the sample branch (cup `-0.75`, league `-0.25`, level-ball lean, second leg, etc.) for later unplayed-match inference
5. Check whether the original reasoning matched the intended rule branch
6. Separate execution error from strategy error; separate Track A quality from Track B hit-rate
7. **Skill strict/loose re-check (mandatory every results batch in this workspace):** using this batch + cumulative ledgers, state whether any rule band looks **too strict** or **too loose**; default **no hard-rule change**. Follow `analysis-checklist.md` §9: fill **Lean short table** then **§9 strict/loose table**. Lean W ≠ should have been PLAY; lean L supports non-upgrade. Never treat cover-after-PASS alone as proof of over-strictness.
8. Adjust rules only if a repeatable weakness is visible across multiple results under the **win-rate-first** objective **and** the adjustment gate is met
9. Update `WR-inferred lean` for **similar unplayed** spots using external odds + branch WR + fundamentals as auxiliary modifiers

Recommended adjustment gate:
- `1` result: observation only, no rule change
- `2` similar results: watchlist, no rule change
- `3` similar results in the same rule branch and context: formal review
- `5` comparable settled cases: minimum sample for a local rule tweak
- `8-10` comparable settled cases: preferred sample before a major rule rewrite
- deterministic parsing, settlement, or market-mapping mistakes may be corrected immediately once verified

In post-match review, do not:
- overreact to a single bad beat or lucky win
- loosen the defensive filters just because a rejected favorite covered once
- rewrite the original logic after seeing the scoreline
- rewrite a historical `PASS` into a retroactive `PLAY` after the result; correct future rule text instead, and label the old call as too strict or too loose in a cross-check note
- treat a weak-observation side that later wins as proof the observation should have been a default `PLAY`
- skip the §9 strict/loose block because “nothing to change”
Valid strategy-adjustment signals include:
- repeated failure of the same rule branch under similar season-phase conditions
- recurring misread of motivation or lifecycle mismatch inputs
- consistent evidence that a line type or source hierarchy is being interpreted incorrectly

If the new result is only one isolated sample, keep the strategy unchanged and log it as observation only — **still** write the short strict/loose re-check.

## Decision Workflow

### 0. Match Identity Gate (ask user first if uncertain) · Grill 2026-08

Before any external-odds search is treated as authoritative, and before any table-A / A′ formal direction that cites external lean, lock fixture identity.

#### What “match” means (identity — **not** odds equality)

External page must align on:

| Check | Rule |
|-------|------|
| **兩隊** | Both clubs match user card (via English names / alias table) |
| **賽事類型** | League vs cup vs friendly / super cup / youth, etc. |
| **開賽日** | Same calendar day as user card date (± timezone OK; wrong day = fail) |
| **主客** | **High-risk only** (below); conflict → ask user |

**Do not** treat “external 1X2 ≠ HKJC AH water” as identity failure. User pastes **HKJC execution**; external is for **who is playing + direction/tier**.

#### Alias table

- Path: **`record/hkjc-name-alias.md`**
- **Before** web search: look up HKJC Chinese (and `FBxxxx` if present)
- **Write:** only after **user confirms** a mapping (or user says 鎖名); Agent **must not** auto-append guesses
- Rows marked `ambiguous` → always ask user

#### High-risk (force home/away check) — list ∪ trigger

**List:** two-legged euro / neutral venue; cup; highly ambiguous Chinese (`學生隊`, `國際…`, `維京…`, etc.); U20 / women / B-team; derbies with confusable names.  
**Trigger:** ≥2 plausible external fixtures, **or** external home/away disagrees with card left=home.

#### Per-fixture hard stop (not whole card) — **Q1=A**

When English names / teams / competition / date (or high-risk home-away) **do not lock**:

1. **That fixture only:** no external 1X2 attach; no table-A / A′ direction that depends on external lean  
2. **Ask the user immediately:** 1–2 candidate English names + why uncertain (+ FB code if any)  
3. **Rest of card continues** dual screen  
4. Label fixture: **`ID hold`**

After user confirms → re-fetch external for **that** fixture only → may upgrade to **`ID locked`**.

#### Identity status labels (output)

| Status | Meaning | External lean / table A–A′ using external |
|--------|---------|------------------------------------------|
| **`ID locked`** | Teams + competition + date OK (+ H/A if high-risk) | Allowed |
| **`ID hold`** | Asked user / waiting | **Forbidden** external attach |
| **`incomplete`** | Identity OK or structure-only, but **no usable external** | Structure PASS/lean only; no fake Sources |

#### While waiting (`ID hold`)

- do **not** formal `PLAY` / A′ that needs external Medium/clear lean  
- do **not** attach external 1X2/AH from unverified candidates  
- do **not** put the leg in a parlay  
- may still apply **structure-only** rules (deep line PASS, D-tier, pure -1 reject) without naming external lean  
- list candidate mappings briefly

Wrong-match external data **invalidates that fixture’s external screen** (same severity as settlement mapping bugs).

### 1. Start From Win-Rate Default

Begin by asking which side/line has the **best justified win-rate** under Track B (external lean + score path + branch history).

- Prefer that line as the candidate recommendation.
- Use auxiliary filters (price, cup noise, fundamentals) only to adjust confidence or, if they destroy the WR case, fall back to `PASS`.
- Default to `PASS` when no clear WR edge exists (coin-flip, broken identity, insufficient data) — not merely because the price looks thin.

### 1b. Self Cross-Check (similar settled spots)

After a provisional market lean forms (and before locking Track A `PLAY`), run **Self Cross-Check Against Similar Settled Spots** when required by that section.  
At minimum for batches and borderline calls: assign a branch tag, cite comparables from `post-match-review-grok.md`, state similarity, and separate Track A vs Track B implications.

### 1c. Workspace execution checklist (this repo)

Before locking formal `PLAY` or a confident multi-match `PASS` screen in this workspace, apply **`analysis-checklist.md`** (upgrade gates, force-PASS bands, watchlist).  
This step is **execution discipline** for the local ledger workflow—it does **not** replace the identity gate, invent odds, or elevate capital filters above win-rate.

### 2. Apply the Pre-Match Filter Matrix

#### Lifecycle Mismatch

Trigger this rule when:
- the favorite is in off-season or pre-season form
- the underdog is in full mid-season competitive rhythm

Then:
- treat the favorite's reputation as a market bubble alert
- reject deep favorite handicap positions
- prefer the underdog on the handicap line

#### Season-End Motivation Distortion

Trigger this rule when:
- the match is in the last three rounds of the league
- the favorite has little or no motivation
- the underdog has strong survival, pride, or home-farewell motivation

Then:
- blacklist match-winner betting based only on normal strength data
- avoid simple favorite win assumptions

### 3. Apply Fundamental Friction Filters

#### Personnel Integrity Trigger

Trigger this rule when:
- a side is missing two or more structural spine pieces such as the main striker, first-choice goalkeeper, or primary playmaker
- or one elite absence clearly changes how that side creates goals, protects the box, or progresses possession

Then:
- downgrade confidence in that side's baseline rating
- do not assume the pre-match reputation still deserves the same handicap strength
- avoid backing that side on `-1` or deeper handicap positions unless the external benchmark still shows unusual value after the downgrade

#### Home and Away Venue Friction

Trigger this rule when:
- the home team has a meaningful normal home-ground advantage
- or the away team is materially weaker in away conditions than at home

Then:
- treat the venue as a real but secondary structural edge
- use it as a supporting factor only when the external benchmark already leans in the same direction
- do not upgrade a weak market read into a bet solely because of the venue factor

#### Logistics and Travel Tax

Trigger this rule when the away side faces one or more of these conditions:
- travel distance above roughly `3000 km`
- time-zone shift of around `3` hours or more
- schedule compression such as `3` matches in `7` days

Then:
- treat fatigue and preparation loss as a material risk
- avoid backing that away side on aggressive handicap positions, especially `-0.75` or deeper
- if the bet case depends heavily on late-match control from the traveling favorite, default toward `PASS`

#### Cup Tactical Phase

Trigger this rule when:
- the match is a two-legged knockout tie
- and it is the first leg

Then:
- assume stronger away teams may prioritize control, damage limitation, and preserving the second leg rather than forcing a margin win
- avoid backing the stronger away team on deep handicap lines by default
- only consider an away favorite handicap if the line is still shallow and the external benchmark clearly supports it

#### Cup Favorite Medium-Deep Handicap Zone (auxiliary modifier)

Apply to single-leg cup ties (national cup, continental knockout single leg, Australia Cup, Korea Cup, and similar), not only two-legged ties.

When the quoted handicap is about `-0.75` to `-1`:
- **Primary:** estimate Track B WR (e.g. `-0.75` wins on win-by-1; settled branch has mixed W/L).
- **Auxiliary:** cup rotation, motivation, and variance **lower confidence**; may push toward shallower dog buffer or `PASS` if WR edge is not clear after modifiers.
- Do **not** auto-reject solely because “cup habit PASS” if external lean + WR path + branch history still favor the line.

**External strength tier (required in self cross-check for this branch):**

Classify the favorite’s external 1X2 (or sharp AH) lean before stating WR confidence:

| Tier | Typical 1X2 band (guide) | WR-first default |
|------|--------------------------|------------------|
| **Strong gap** | favorite win roughly ≤`1.55` and clear gap vs draw/dog | Medium WR lean allowed only if not second-leg distortion and branch not pure kill history; still case-by-case |
| **Medium gap** | favorite roughly `1.55`–`1.85` | Mixed branch → usually **Low** WR confidence; lean only if fundamentals clean |
| **Light gap** | favorite roughly `>1.85` or near-even | Treat as weak WR edge → prefer `PASS` or dog buffer, not cup fav `-0.75`/`-1` |

Also state in the cross-check block: first vs second leg, aggregate if any, and whether settled `cup-fav-0.75` / `cup-fav-1.0` is mixed / kill-heavy / cover-heavy.

- `-1` is harsher than `-0.75` on Track B (win-by-1 = push, not W) — require a stronger external tier than `-0.75` for the same confidence.
- Never upgrade confidence solely because a past cup favorite covered after a historical `PASS`.

If only incomplete external 1X2 is available, lower WR confidence rather than inventing cover certainty.

### 4. Apply Odds and Handicap Thresholds

#### Anti-Low-Value Rule (auxiliary)

**Role under win-rate-first:** soft warning / confidence downgrade on extreme deep favorites — **not** an automatic kill of a strong Track B cover case unless WR edge is unclear.

Flag when:
- primary-market direct win odds are known and below `1.45`
- and the handicap line is `-1.5` or deeper

Reason (auxiliary capital note):
- payout is poor even when WR is decent; still report WR path honestly
- may prefer a shallower line with similar or better WR if available

If primary-market direct win odds are unavailable:
- do not guess or backfill them from a different market
- do not let HKJC alone impersonate full external confirmation
- mark the price filter as `not fully checked`

#### League Shallow Favorite `-0.25` (WR-first soft band)

Applies to **league** (or stable domestic league context), not cup/second-leg distortion spots.

Track B path for favorite `-0.25`:
- win → full W
- **draw → half-win → W**
- lose → L

Settled local samples have often been WR-friendly on this line. Under win-rate-first:

- Lead with the WR case when external 1X2 shows a **clear** favorite lean (not near-even).
- **Do not reject a clear WR-preferred `-0.25` solely because the price is “thin” or “stable-profit insufficient.”** Thin price is auxiliary only (confidence / stake note).
- Legitimate `PASS` reasons on this band: weak or missing external lean, true near-even, identity/data incomplete, fundamentals that materially cancel the lean, or WR edge not actually clear after modifiers.
- If `PASS` despite a clear external lean + clean structure, state that the WR case was weak/absent — do not use capital wording as the only veto.
- Confidence still capped by sample size and league noise; this is not “always PLAY every league `-0.25`.”

#### Underdog Handicap Optimization

For a single bet:
- prefer `+1` or `+1.25`
- prioritize push or half-loss protection over higher upside
- `+0.75` is allowed when external lean + price + structure support it; it is not banned because Track B treats lose-by-1 as Loss

For a parlay:
- prefer `+0.5`
- do not use `+1` in parlays when a push would reduce that leg to `1.00`
- avoid void dilution that weakens real parlay progression

Favorite `-0.75` on Track A remains case-by-case (cup reject zone often blocks); on Track B, win-by-1 counts as Win for ledger/inference only.

#### Level-Ball / PK External-Lean Overlay

HKJC (or other user-quoted) level ball / 平手 / PK is not automatically a coin-flip blacklist.

Split the cases:

1. True coin-flip: external 1X2 is symmetric or mixed with no usable lean, and no structural edge exists → keep absolute blacklist `PASS`
2. External lean on a near-even market: external 1X2 does not support the home side as favorite, or mildly prefers the away/underdog side, while the executable line is still level ball or a very shallow underdog line → the external-preferred side on level ball may be a defensive `PLAY` candidate under Underdog Handicap Protection

Execution rules for case 2:
- form direction only from the external benchmark first; never from HKJC level ball alone
- do not bias toward the home side merely because HKJC opened 平手
- if external implies away/underdog is not weaker (or is slightly stronger) and HKJC still prices level ball, prefer the external-lean side on 平手 for **win-rate** (draw pushes)
- keep confidence at most `Medium`; if the external lean is tiny, noisy, or source-quality is weak, stay `PASS`
- this is a WR overlay when lean exists, not a license to bet every balanced cup or league derby
#### O/U Qualification Rules

Only consider a full-match O/U bet when the total-goals environment is reasonably predictable.

Allow O/U analysis when most of these are true:
- the match is in a stable league context rather than pre-season, exhibition, or heavily rotated cup conditions
- both teams' style, pace, and game-state tendencies point in a compatible direction for `Over` or `Under`
- motivation is clear and likely to affect tempo in a predictable way
- team news, weather, pitch conditions, and defensive absences support the same total-goals view rather than cancel each other out
- the primary-market O/U line is broadly consistent with other external reference markets, even if HKJC differs on price
- the O/U view is supported by at least two independent reasons, not just recent scorelines

Force `PASS` on O/U when any of these apply:
- the match is pre-season, a friendly, or otherwise structurally noisy
- likely tempo control is unclear or both teams can plausibly force opposite scripts
- the total-goals case depends too heavily on one fragile assumption such as an early goal, one missing defender, or one hot scoring streak
- the primary-market line is sitting on a sensitive threshold and cross-book comparison does not show a clear edge
- the user only has superficial trend data such as recent overs or unders, without a structural explanation

O/U-specific stable-profit rules:
- prefer O/U only when the total-goals edge is clearer than the handicap edge
- do not recommend O/U solely because the match-winner or handicap side looks unattractive
- keep O/U confidence capped at `Medium`
- if the total-goals read is plausible but not robust, return `PASS`

### 5. Apply the Parlay Execution Rules

If building a parlay:
- keep the structure to `2-leg` or `3-leg` parlays only
- do not force extra legs to chase payout
- do not build a parlay unless each leg would be close to acceptable as a standalone defensive position

Preferred composition:
- one strength-gap leg: a mid-season strong side against clearly weaker international cup opposition, with handicap no deeper than `-1.5`
- one market-bubble underdog leg: a season-ready underdog benefiting from inflated brand bias on the favorite, ideally at `+0.5`

### 6. Enforce Absolute Market Blacklists

- balanced or coin-flip matches **only when external pricing shows no usable lean** (and no side has a strong structural edge that survives external silence): both teams similar cycle state **and** external 1X2/AH is symmetric or mixed with no readable direction
- **do not** treat equal executable AH prices alone (e.g. `1.88 / 1.88`) as proof of true coin-flip without external check
- do not expand this blacklist to every level-ball or near-even quote: if external 1X2 rejects home advantage or mildly prefers one side, apply `Level-Ball / PK External-Lean Overlay` (or WR-path on the lean side’s shallow line) instead of forcing `PASS` by habit
- historical local lesson (牙山): over-applying coin-flip PASS when external mildly leaned away was too strict; keep true-no-lean PASS, allow lean-side level-ball evaluation

## Decision Priorities

When rules conflict, use this priority order:
0. match identity gate (hard — ask user if uncertain; block `PLAY` until locked)
1. **win-rate edge** (Track B: external lean + line settlement path + comparable branch WR)
2. true coin-flip / no usable lean → `PASS` (no WR edge)
3. auxiliary modifiers: fundamentals, cup/second-leg noise, logistics, motivation, anti-low-value price warning
4. level-ball external-lean overlay and underdog buffer choice (prefer lines with better WR paths, e.g. `+1` over thin `+0.75` when similar); league shallow `-0.25` WR band when external lean is clear
5. cup external gap tiering for medium-deep favorites (soft; does not auto-kill clear WR)
6. parlay construction preferences
7. soft capital notes (thin price, variance) — annotate; do not override a clear WR-preferred line without stating that WR edge collapsed

If no side has a usable win-rate edge, keep the final answer as `PASS`.

## Output Format

Use this structure in the final response:

```markdown
Decision: PASS | PLAY
Market: [recommended market or "none"]
Primary lens: Win-rate (Track B)
WR case: [why this line has the best justified win-rate; score paths that are W/L]
Fundamentals (mandatory snapshot — not optional flavor):
- Motivation / season phase: ...
- Personnel: ...
- Home-away / travel / schedule: ...
- Style / draw-magnet risk: ...
- Veto or support: [blocks PLAY | lowers conf | supports lean]
Source Basis: External Primary | HKJC Input + External Benchmark | HKJC Only Incomplete Value Screen
Sources Used: [named public sources actually checked, or `none`]
Mode: Single | Parlay
Confidence: Low | Medium
Auxiliary notes: [price / cup / capital caveats — secondary; fundamentals already above]

Rule Hits:
- [rule triggered]

Reasoning:
- [lead with WR + external; then fundamentals modifier; then other aux]

Self Cross-Check:
- Branch: [tag]
- External gap tier (cup-fav): Strong | Medium | Light | n/a
- Comparables: [settled spots + W/L/P]
- Similarity: High | Medium | Low
- Implication (WR primary / aux filters): [...]

O/U Check:
- [use only when O/U is considered]

Rejected Options:
- [lower-WR or inferior lines]

Risk Notes:
- [what could still go wrong]
```

When the user is giving a post-match result, use this review structure instead:

```markdown
Original Decision: PASS | PLAY
Original Market: [original quoted market]
Actual Result: [match score and settlement against the original quoted line]
Track B (WR ledger): W | L | P  (half-win=W, half-loss=L, push=P excluded from WR)
Branch Tag: [e.g. cup-fav-0.75 | league-0.25 | level-ball-lean | second-leg]
Assessment: Strategy Held | Execution Error | Review Needed
Sample Status: Observation | Watchlist | Formal Review | Eligible for Rule Change

What Was Correct:
- [rule or read that still holds]

What Was Wrong:
- [specific input read or rule application issue]

WR inference note (optional):
- [comparable unplayed spots: external odds + fundamentals + this branch WR; not auto-PLAY]

Adjustment Decision:
- Keep strategy unchanged | Watch for repeat pattern | Adjust rule

### Lean 短表（本批 · 必填 · checklist §9a）
| 結構 | 場次 | 賽前 | Track B | 對升格含義 |
|------|------|------|---------|------------|
| 貼線 -0.75 | … / 無 | … | … | … |
| 歐戰次回合 -0.75 | … / 無 | … | … | … |
| Strong +1 lean | … / 無 | … | … | … |

### Skill 嚴寬簡審（本批 · 必做 · checklist §9）
| 維度 | 本批訊號 | 累計含義 | 動作 |
|------|----------|----------|------|
| 過嚴？ | … | … | 維持 / watch / checklist |
| 過寬？ | … | … | 維持 / 執行收緊 / watch |
| Lean 分層 | … | … | 不回寫 / 防漏候選 |
| 改 skill 硬規則？ | 否（預設） | … | 僅 formal review 時待議 |
```

## Quality Checks Before Finishing

Before returning a recommendation, verify that:
- each fixture has **ID locked | ID hold | incomplete** where external is used; never `PLAY` on a guessed club mapping
- identity checks are **teams + competition + date** (+ H/A if high-risk) — **not** HKJC vs external price equality
- any external odds cited refer to the same verified teams, competition, and leg as the user input
- alias table consulted when present; no silent auto-write of aliases
- Sources are real or explicitly `none`; conflicting external directions → incomplete lean
- when self cross-check was required, a branch tag and named comparables appear; low-similarity history was not used to force PLAY
- level-ball-lean history was not applied to true near-even markets
- Track B win-rate case is stated first; auxiliary capital/price/cup notes are labeled secondary
- **Fundamentals snapshot present** on any formal PLAY (and preferred on WR lean); rotation/dead-rubber/structural absences that kill the WR case → not PLAY
- in this workspace for pre-match **odds cards**: dual output present (defensive + streak-roll), and `analysis-checklist.md` considered for formal PLAY upgrade vs PASS / WR-lean-only (especially light `-0.25`, pure `-1`, deep favorites, formal `-0.75` stating draw=L + fundamentals, and **§1d 防漏升** so clear shallow/dog structures are not silent PASS)
- the answer explicitly states `PASS` or `PLAY` (or an identity-hold asking the user, which is not a formal bet decision)
- the market type matches the correct branch for single or parlay mode
- the main recommendation is driven by external benchmark prices whenever they can be obtained
- any HKJC prices are used only as a relative reference, execution overlay, or line-mapping note
- unlabeled user input in this workspace is not mistakenly treated as external odds
- HKJC-only user input is primarily being evaluated for investment value against external prices, not used as the core directional signal by default
- any rejection is tied to a named rule
- the recommendation leads with **win-rate (Track B)**; price/capital/cup notes are labeled auxiliary
- a clear WR-preferred line is not rejected solely for “thin price” or legacy stable-profit habit without showing WR edge is weak
- **league favorite `-0.25` with clear external lean** is not rejected solely for thin price / stable-profit wording
- **cup favorite `-0.75`/`-1`** states external gap tier (strong/medium/light) and mixed-branch honesty when self cross-check runs
- shorthand-input analysis without direct win odds is labeled as incomplete screening, not a fully cleared recommendation
- deep favorite lines are judged on WR cover paths first; lifecycle/personnel/travel act as confidence modifiers
- anti-low-value and cup medium-deep zones are soft/auxiliary unless WR edge is absent
- missing primary-market direct win odds are disclosed; do not invent them
- HKJC-only analysis does not pretend to have external confirmation when no benchmark could be obtained
- true coin-flip / no lean → `PASS`; level-ball with clear external lean may be WR-playable
- O/U recommendations are made only when the total-goals environment is structurally predictable
- O/U recommendations are not based only on recent overs, recent unders, or surface scoring trends
- post-match reviews do not force a rule change from a single isolated result
- historical `PASS` decisions are not rewritten into retroactive `PLAY` after settlement

## Response Style

Keep the analysis readable and operational:
- if identity is uncertain, lead with a clear ask to the user (candidate names + what to confirm); do not bury the question under a full faux analysis
- **lead with win-rate case** (which line, which score paths are W/L under half-win=W / half-loss=L)
- then external market lean that supports that WR case
- **always write a Fundamentals snapshot** on PLAY / borderline lean (motivation, personnel, travel, rotation, draw risk); odds-only screens are incomplete
- treat fundamentals as **modifiers/vetoes** (not side-inventors); cup noise and price remain auxiliary
- mention HKJC mainly as execution overlay, not directional driver
- when self cross-check runs, keep the comparables block short and explicit (branch, 2–5 cases, similarity, WR implication)
- if discussing O/U, separate structural tempo reasons from simple recent-score trends
- avoid giving multiple conflicting bet options
- if no usable WR edge, end with `PASS`
