---
name: defensive-betting-analysis
description: 'Analyze football betting opportunities with a defensive, capital-protection-first strategy where external odds such as Pinnacle and bet365 drive the betting direction, while Hong Kong Jockey Club prices are usually user-supplied input used to judge whether the available HKJC line is worth investing in relative to the external benchmark. Includes dual-track capital vs win-rate ledger and self cross-check against similar settled spots in post-match reviews. Football only. Use when evaluating 足球 or football matches, external bookmaker lines, Asian handicap lines, 讓球, 受讓, 串關, 賽前過濾, cross-book comparison, self cross-check, historical branch comparison, odds thresholds, season-phase mismatch, motivation distortion, blacklisted markets, synthetic handicap conversion, post-match result review, or whether the correct action is PASS.'
argument-hint: 'Preferred detailed input format: 隊伍A勝賠率 隊伍A 隊伍A讓球賠率 讓球盤 隊伍B讓球賠率 隊伍B 隊伍B勝賠率. In this workspace, treat unlabeled quoted prices as HKJC prices unless they are explicitly labeled as external-market prices; then seek Pinnacle, bet365, or other major external benchmarks to determine direction. Use *讓球盤 when the handicap is synthetically converted from HKJC 3-way handicap.'
user-invocable: true
---

# Defensive Betting Analysis

## What This Skill Does

This skill turns a compact betting ruleset into a readable decision workflow for football match analysis.

This skill is for football only. Do not apply it to basketball, baseball, tennis, esports, or other sports.

Build the betting view from external market prices first, preferably Pinnacle, other sharp or market-making books, and then bet365 or similar major references. Use Hong Kong Jockey Club mainly as the user-supplied investment price to judge whether the quoted HKJC line is worth taking relative to that external benchmark.

Whenever the user provides only HKJC or unlabeled prices, the analysis should actively seek accessible external public odds or public odds summaries before forming a directional view. If such a public source is used, the final write-up must explicitly name it.

If the user provides only external odds, do not force the analysis back into a Hong Kong Jockey Club-first framing.

For this workspace and user workflow, if odds are pasted without a source label, treat them as HKJC prices rather than external odds. The default next step is to look for external benchmark prices and use those benchmarks, not the HKJC line itself, to determine the preferred betting direction.

Do not claim external confirmation unless a concrete public source was actually checked. Never imply unseen Pinnacle, bet365, or consensus support.

It is intentionally conservative:
- Stable profit is more important than payout maximization.
- Capital protection is more important than aggressive yield chasing.
- The default action is `PASS` unless the market clearly fits the rules.
- Deep favorite handicaps, low-value prices, chaotic matches, and weak-information spots should be rejected.

Every recommendation must clear a stable-profit standard:
- do not recommend a bet only because it looks survivable
- prefer repeatable, lower-variance edges over occasional high-payout shots
- if the edge is unclear, fragile, or depends on too many assumptions, return `PASS`

## Dual Track: Capital Decision vs Win-Rate Ledger

This workspace runs **two parallel tracks**. Do not collapse them into one KPI.

### Track A — Capital decision (`PLAY` / `PASS`)

- Purpose: whether real money should be risked under the defensive framework.
- Drivers: external odds direction, HKJC execution overlay, capital protection, stable-profit filters, blacklists, cup reject zones, identity gate.
- High win-rate alone does **not** force `PLAY` (low price / high variance / cup noise can still be `PASS`).

### Track B — Win-rate ledger (no profit weighting)

- Purpose: measure **handicap settlement hit rate** and calibrate **line-push / similar-spot inference** for fixtures not yet played.
- Ignores odds magnitude and staking returns for the score itself.
- Settlement convention (mandatory for this ledger):
  - **half-win (贏半) = Win**
  - **half-loss (輸半) = Loss**
  - **full win / full loss** as usual
  - **push / void (走盤)** = exclude from win-rate denominator (or report push rate separately; never count as Win)

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

### Using Track B for unplayed matches (line push / inference)

When inferring whether a **similar unplayed** market is more likely to settle Win under Track B:

1. **External odds first** — 1X2 / AH lean and implied strength gap; do not invent lines.
2. **Settled WR ledger** — only comparable branches (same band: e.g. cup favorite `-0.75`, league shallow `-0.25`, second-leg trailing home, NPL underdog `+0.75`).
3. **Fundamentals (基本面)** — required, not optional: season phase, motivation, personnel integrity, home/away, travel, cup leg state, rotation risk, style matchup. High WR in a different structural context does not transfer.
4. **Output as inference, not auto-PLAY** — may state `WR-inferred lean` / `line-push note` for a candidate handicap; capital `PLAY` still requires Track A filters.
5. **Never** use Track B alone to override Force-PASS, identity failure, true coin-flip blacklist, or cup favorite reject zone for staking.

### Do not over-filter half-lines on Track A

- `±0.75` is **not** an automatic Track A blacklist.
- Reject `-0.75` / `+0.75` only when a named rule applies (cup favorite reject zone, anti-low-value, thin stable-profit, noise/incomplete benchmark, reverse of external lean, etc.).
- Track B may still **log** those lines for WR even when Track A is `PASS`.

## Self Cross-Check Against Similar Settled Spots

Before finalizing Track A `PLAY`/`PASS` (and when writing Track B `WR-inferred lean`), **run a self cross-check**: compare the current fixture to **comparable settled samples** in this workspace, then adjust confidence or decision only when the comparison is structurally valid.

Primary local sources (in order):
1. `post-match-review-grok.md` (and any dated post-match review files in the repo root)
2. Branch tags / Track B ledger tables already written in those reviews
3. Optional: `post-match-review-gpt.md` or other agent reviews for disagreement notes — never copy a decision without re-checking identity and external odds

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
   - external lean and strength of lean
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

6. **Reconcile to dual track**
   - Track A: cross-check may **confirm PASS**, **block a fragile PLAY**, or rarely **support PLAY** when history + external + fundamentals align with stable-profit rules.
   - Track B: cross-check feeds `WR-inferred lean` strength (`Low` if n small, noise high, or similarity low).
   - If history is mixed (branch has both W and L), default to **caution** and say so.

7. **Write a short cross-check block in the analysis** (required when step was run)

```markdown
Self Cross-Check:
Branch: [tag]
Comparables: [2–5 named settled spots + W/L/P or PASS outcome]
Similarity: High | Medium | Low
Implication (Track A): confirm PASS | block PLAY | support PLAY (only if rules still clear)
Implication (Track B): WR-inferred lean / none; confidence Low|Medium
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

| Branch idea | Track A bias | Track B note |
|-------------|--------------|--------------|
| Cup favorite `-0.75` to `-1` | Default PASS | Mixed W/L; covers do not loosen reject zone |
| League strong favorite `-0.75` | Case-by-case; often PASS if thin | Win-by-1 = W |
| Shallow league `-0.25` | Often PASS if thin price | Draw = W for favorite |
| Level ball + clear external lean | Possible PLAY (overlay) | Needs lean; not true coin-flip |
| True / near coin-flip PK | PASS | No single-side push |
| Aus semi-pro / NPL dog shallow | Cautious; prefer deeper buffer historically | High variance; small n |
| Aus semi-pro dog `+1` | More defensive-friendly than `+0.25` | Push possible at exact 1-goal loss |
| Second leg / aggregate | PASS bias if state distorted | Single-match W ≠ tie outcome |

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

## Market Source Policy

Use these source layers:
- primary analysis source: external bookmaker odds, with preferred benchmark order of Pinnacle, other sharp or market-making books, then bet365, then other major books
- secondary reference source: Hong Kong Jockey Club prices for relative comparison, local-bias checking, and optional execution mapping

Apply these scope rules:
- for this workspace, if the source is not stated, assume the user is giving HKJC prices rather than external prices
- when the user gives only HKJC prices, actively seek an accessible external benchmark before forming a directional view
- if the user provides external odds, treat them as the main pricing signal unless explicitly told otherwise
- if the user provides multiple external sources, prioritize sharp or consensus pricing over isolated soft-book prices
- **if team names, competition names, kickoff context, home/away, leg (first/second), or matchup identity are uncertain, stop and ask the user first** before continuing analysis or attaching external odds; do not guess among lookalike clubs
- use HKJC only after the external direction is formed; HKJC may improve or worsen execution quality, but should not become the core reason for a bet by itself
- if the user provides only non-HKJC prices, continue normally and label the result as external-market analysis
- if the user provides only HKJC prices and an external benchmark is found, label the result as HKJC input plus external benchmark analysis
- if the user provides only HKJC prices and no external benchmark can be obtained, analysis is still allowed, but clearly mark the view as incomplete value screening rather than a fully benchmarked directional read
- when market naming is ambiguous, keep the recommendation anchored to the handicap or total-goals context already provided by the user, and still ask the user if team identity is not locked
- when any external public benchmark is used, identify the exact source in the response instead of referring vaguely to `external market`
- if no external public source was successfully checked, say so plainly and downgrade to incomplete screening or `PASS`
- never attach external 1X2/AH from a different match, club, or competition to an HKJC line; wrong-match odds make the whole screen invalid

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
- every final recommendation that uses an external view must include a `Sources Used` line naming the actual public pages checked

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

The preferred detailed one-line input format is:

```text
隊伍A勝賠率 隊伍A 隊伍A讓球賠率 讓球盤 隊伍B讓球賠率 隊伍B 隊伍B勝賠率
```

Default source assumption for this workspace: when the user posts unlabeled prices in this format, treat them as HKJC prices. Only treat them as external primary-market prices when the user explicitly labels an external source or provides cross-book context.

Analytical source assumption for this workspace: unlabeled quoted prices are usually the HKJC price the user can actually bet. The skill should then seek external benchmark prices separately and use those external prices to decide direction before judging whether the HKJC quote is investable.

Interpret it as:
- `隊伍A勝賠率`: side A quoted match-winner odds
- `隊伍A`: side A
- `隊伍A讓球賠率`: side A quoted handicap odds
- `讓球盤`: the handicap line, always from side A perspective
- `隊伍B讓球賠率`: side B quoted handicap odds
- `隊伍B`: side B
- `隊伍B勝賠率`: side B quoted match-winner odds

Example:

```text
1.62 Liverpool 1.86 -1.25 2.02 Burnley 5.20
```

If the handicap is synthetically converted from Hong Kong Jockey Club 3-way handicap, mark it with a leading `*`:

```text
1.62 Liverpool 1.86 *-1.5 2.02 Burnley 5.20
```

Fallback shorthand is accepted only for incomplete screening when direct win odds are unavailable:

```text
隊伍A 隊伍A讓球賠率 讓球盤 隊伍B讓球賠率 隊伍B
```

Parsing rules:
- treat the detailed format as the default contract whenever available
- treat this as a handicap-market input unless the user states otherwise
- treat both handicap odds as the user's quoted source; if unlabeled in this workspace, default them to HKJC prices
- treat both win odds as the user's quoted source; if unlabeled in this workspace, default them to HKJC prices
- **if the team names or matchup mapping are unclear or only medium-confidence, ask the user to confirm the exact teams, competition, and leg before any directional recommendation or external-odds attach**
- do not resolve ambiguous HKJC Chinese transliterations by picking the first web hit (example failure mode: treating `國際杜古` as Inter Club d'Escaldes instead of Inter Turku)
- when the quoted source is HKJC-only, try to obtain external benchmark prices before turning the screen into a directional recommendation
- the handicap line is always side A's line: negative means side A gives goals, positive means side A receives goals
- infer the favorite and underdog from side A's handicap direction first, then from market pricing if needed
- if the handicap line begins with `*`, treat it as a synthetic handicap reference converted from Hong Kong Jockey Club 3-way handicap
- if HKJC has no Asian handicap line and the user converts a Hong Kong Jockey Club 3-way handicap market into a handicap line, require the `*` marker when possible
- synthetic HKJC handicap references are allowed for screening, but lower confidence because 3-way handicap settlement is not identical to Asian handicap settlement
- if original 3-way handicap prices are available, mention them as HKJC source context instead of pretending they are true Asian handicap odds
- if the user supplies both external and HKJC prices, form the main read from the external prices first and use HKJC only as an overlay
- if the user supplies only HKJC prices, analysis is still possible, but first attempt to benchmark those prices against Pinnacle, bet365, or another major external source
- if the user supplies only HKJC prices and no external benchmark is obtainable, do not present the result as a fully benchmarked directional edge
- if benchmark attempts succeed only through public summaries or public odds portals rather than direct bookmaker pages, state that limitation explicitly and name those public sources
- if the user later clarifies that quoted prices are external, immediately switch the main pricing source to that external market context
- if the shorthand format is used, treat the output as incomplete screening rather than a full recommendation unless separate win odds are provided
- if the shorthand format is used without separate win odds, mark the direct-win filter as missing and do not present the result as a fully cleared `PLAY`
- if season phase, motivation, or match context is missing, ask for it or mark the final view as incomplete
- if the line cannot be parsed cleanly, stop and request a corrected format instead of guessing

## Post-Match Review Workflow

When the user provides a result after the match, use it for strategy review rather than for outcome chasing.

Review in this order:
1. Restate the original decision: `PASS` or `PLAY` (Track A)
2. Record the actual market outcome against the original quoted line (cash / standard AH if needed)
3. **Log Track B win-rate settlement** for the primary quoted handicap and any key alternative line: `W` / `L` / `P` under half-win=Win, half-loss=Loss
4. Tag the sample branch (cup `-0.75`, league `-0.25`, level-ball lean, second leg, etc.) for later unplayed-match inference
5. Check whether the original reasoning matched the intended rule branch
6. Separate execution error from strategy error; separate Track A quality from Track B hit-rate
7. Adjust Track A strategy only if a repeatable rule weakness is visible across multiple results (WR hits alone do not force looser PLAY filters)
8. Optionally update a short `WR-inferred lean` note for **similar unplayed** spots using external odds + fundamentals + branch WR — still not auto-PLAY

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
Valid strategy-adjustment signals include:
- repeated failure of the same rule branch under similar season-phase conditions
- recurring misread of motivation or lifecycle mismatch inputs
- consistent evidence that a line type or source hierarchy is being interpreted incorrectly

If the new result is only one isolated sample, keep the strategy unchanged and log it as observation only.

## Decision Workflow

### 0. Match Identity Gate (ask user first if uncertain)

Before any external-odds search is treated as authoritative, and before any `PLAY` or confident directional `PASS` narrative, lock fixture identity:

Required identity locks when available:
- official English (or local) club names for both sides
- competition and country/region
- home/away
- date / kickoff context if needed to disambiguate
- for two-legged ties: first leg or second leg, and prior-leg score if known

**If identity is uncertain, stop and ask the user first.** Prefer a short confirmation question over guessing.

Ask when any of these are true:
- HKJC Chinese names admit multiple plausible clubs (for example `國際…`, `夏德`, `維京…`, similar transliterations)
- search results point to two different fixtures or two different competitions
- home/away or which leg is unclear
- external pages found do not clearly match both user-quoted names
- confidence in the mapping is less than high

While waiting for confirmation:
- do **not** issue a formal `PLAY`
- do **not** attach external 1X2/AH from a candidate match that is not verified
- do **not** put the leg in a parlay
- you may list candidate mappings and why they conflict
- if the user needs an interim stance, use incomplete screening / provisional `PASS` only, and label identity as unconfirmed

After the user confirms identity, re-fetch external benchmarks for **that** fixture only, then continue the normal filters.

Wrong-match external data invalidates the entire screen for that line. Treat this as an execution error class equal in severity to a settlement/mapping bug.

### 1. Start From a Defensive Default

Begin with the assumption that the match is a `PASS`.

Only move away from `PASS` if the matchup survives every filter below.

Only recommend `PLAY` when the setup supports both capital protection and a realistic stable-profit edge.

### 1b. Self Cross-Check (similar settled spots)

After a provisional market lean forms (and before locking Track A `PLAY`), run **Self Cross-Check Against Similar Settled Spots** when required by that section.  
At minimum for batches and borderline calls: assign a branch tag, cite comparables from `post-match-review-grok.md`, state similarity, and separate Track A vs Track B implications.

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

#### Cup Favorite Medium-Deep Handicap Reject Zone

Apply to single-leg cup ties (national cup, continental knockout single leg, Australia Cup, Korea Cup, and similar), not only two-legged ties.

Default to `PASS` on the cup favorite when the quoted handicap is about `-0.75` to `-1` (inclusive of common half-lines in that band), unless a strong external benchmark still supports the favorite handicap after cup noise is considered.

Reasons:
- cup rotation, motivation asymmetry, and single-elimination variance inflate favorite-cover risk
- low-to-mid favorite win prices plus a medium-deep handicap rarely meet the stable-profit standard
- settled workspace samples (for example cup home favorites failing at `-1` or `-0.75`) re-validate this reject zone

Do not treat a clear external favorite direction alone as permission to back cup favorite `-0.75` / `-1`. Direction and investable handicap are separate questions.

If only incomplete external 1X2 is available and no usable external AH exists, keep cup favorite `-0.75` to `-1` as `PASS` rather than forcing a cover bet.

### 4. Apply Odds and Handicap Thresholds

#### Anti-Low-Value Rule

Force `PASS` when:
- primary-market direct win odds are known and below `1.45`
- and the handicap line is `-1.5` or deeper

Reason:
- the risk is too high relative to the reward
- this is exactly the kind of favorite pricing the strategy rejects

If primary-market direct win odds are unavailable:
- do not guess or backfill them from a different market
- do not let HKJC alone impersonate full external confirmation
- mark the rule as `not fully checked`
- treat the view as incomplete screening unless the user later supplies the missing win odds
- lower confidence rather than pretending the filter passed

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
- if external implies away/underdog is not weaker (or is slightly stronger) and HKJC still prices level ball, prefer the external-lean side on 平手 when the price clears a stable-profit screen
- keep confidence at most `Medium`; if the external lean is tiny, noisy, or source-quality is weak, stay `PASS`
- this is a capital-protection overlay, not a license to bet every balanced cup or league derby
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

- balanced or coin-flip matches where both teams are in similar cycle state, external pricing shows no usable lean, and no side has a strong structural edge
- do not expand this blacklist to every level-ball quote: if external 1X2 rejects home advantage or mildly prefers one side, apply `Level-Ball / PK External-Lean Overlay` instead of forcing `PASS` by habit

## Decision Priorities

When rules conflict, use this priority order:
0. match identity gate (ask user if uncertain; block `PLAY` until locked)
1. `FORCE PASS` rules
2. market blacklist rules (true coin-flips only)
3. cup favorite medium-deep handicap reject zone and cup first-leg / logistics friction warnings
4. motivation distortion warnings
5. underdog handicap protection rules, including level-ball external-lean overlay
6. parlay construction preferences

If a match triggers any high-risk exclusion and no strong compensating edge remains, keep the final answer as `PASS`.

If a market passes the filters but still does not support a stable-profit profile, keep the final answer as `PASS`.

## Output Format

Use this structure in the final response:

```markdown
Decision: PASS | PLAY
Market: [recommended market or "none"]
Source Basis: External Primary | HKJC Input + External Benchmark | HKJC Only Incomplete Value Screen
Sources Used: [named public sources actually checked, or `none`]
Mode: Single | Parlay
Confidence: Low | Medium
Profitability Profile: Stable | Borderline

Rule Hits:
- [rule triggered]
- [rule triggered]

Reasoning:
- [short explanation tied directly to the rules]

Self Cross-Check:
- Branch: [tag]
- Comparables: [settled spots + outcomes]
- Similarity: High | Medium | Low
- Implication (Track A / Track B): [...]

O/U Check:
- [use only when O/U is considered: why it qualified or why it failed]

Rejected Options:
- [favorite deep handicap, match winner, O/U, etc.]

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
```

## Quality Checks Before Finishing

Before returning a recommendation, verify that:
- match identity is locked at high confidence, or the user was asked and has confirmed; never `PLAY` on a guessed club mapping
- any external odds cited refer to the same verified teams, competition, and leg as the user input
- when self cross-check was required, a branch tag and named comparables appear; low-similarity history was not used to force PLAY
- level-ball-lean history was not applied to true near-even markets
- Track B WR inference is labeled and does not silently replace Track A filters
- the answer explicitly states `PASS` or `PLAY` (or an identity-hold asking the user, which is not a formal bet decision)
- the market type matches the correct branch for single or parlay mode
- the main recommendation is driven by external benchmark prices whenever they can be obtained
- any HKJC prices are used only as a relative reference, execution overlay, or line-mapping note
- unlabeled user input in this workspace is not mistakenly treated as external odds
- HKJC-only user input is primarily being evaluated for investment value against external prices, not used as the core directional signal by default
- any rejection is tied to a named rule
- the recommendation is aligned with stable-profit logic, not merely with actionability
- shorthand-input analysis without direct win odds is labeled as incomplete screening, not a fully cleared recommendation
- deep favorite handicaps are not recommended after a lifecycle mismatch alert
- deep favorite handicaps are not recommended after major personnel-integrity damage, heavy travel tax, or first-leg tactical drag unless the external benchmark is exceptionally supportive
- sub-`1.45` favorite win prices with `-1.5` or deeper handicap lines are rejected when the primary-market direct win price is actually known
- missing primary-market direct win odds are disclosed as an unchecked filter, not silently ignored
- HKJC-only analysis does not pretend to have external confirmation or a benchmarked directional edge when no benchmark could be obtained
- chaotic even matches are not recommended unless the user explicitly asks to override the framework
- true coin-flip blacklists are not applied to level-ball markets where external 1X2 already shows a usable lean against the home side
- cup favorite handicaps around `-0.75` to `-1` default to `PASS` unless external support is unusually strong
- a clear external favorite direction is not treated as permission to buy medium-deep cup favorite handicaps
- O/U recommendations are made only when the total-goals environment is structurally predictable
- O/U recommendations are not based only on recent overs, recent unders, or surface scoring trends
- post-match reviews do not force a rule change from a single isolated result
- historical `PASS` decisions are not rewritten into retroactive `PLAY` after settlement

## Response Style

Keep the analysis readable and operational:
- if identity is uncertain, lead with a clear ask to the user (candidate names + what to confirm); do not bury the question under a full faux analysis
- lead with the external market read when an external benchmark is available and identity is locked
- mention HKJC mainly to explain whether the user-posted HKJC price is good enough to invest in relative to the external benchmark
- explain why a bet does or does not meet the stable-profit standard
- explain why the strategy prefers safety over payout
- when self cross-check runs, keep the comparables block short and explicit (branch, 2–5 cases, similarity, A/B implication)
- if discussing O/U, separate structural tempo reasons from simple recent-score trends
- avoid giving multiple conflicting bet options
- if the edge is unclear, end with `PASS`
