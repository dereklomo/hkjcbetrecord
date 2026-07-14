---
name: defensive-betting-analysis
description: 'Analyze football betting opportunities with a defensive, capital-protection-first strategy where external odds such as Pinnacle and bet365 drive the betting direction, while Hong Kong Jockey Club prices are usually user-supplied input used to judge whether the available HKJC line is worth investing in relative to the external benchmark. Football only. Use when evaluating 足球 or football matches, external bookmaker lines, Asian handicap lines, 讓球, 受讓, 串關, 賽前過濾, cross-book comparison, odds thresholds, season-phase mismatch, motivation distortion, blacklisted markets, synthetic handicap conversion, post-match result review, or whether the correct action is PASS.'
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

## When to Use

Use this skill when the user wants to:
- evaluate a football match before kickoff
- decide whether to bet or pass
- screen a favorite vs underdog handicap market
- choose between a single bet and a parlay leg
- apply a defensive betting framework
- avoid low-value favorites and noisy markets
- review a finished match and decide whether the strategy needs adjustment

## Market Source Policy

Use these source layers:
- primary analysis source: external bookmaker odds, with preferred benchmark order of Pinnacle, other sharp or market-making books, then bet365, then other major books
- secondary reference source: Hong Kong Jockey Club prices for relative comparison, local-bias checking, and optional execution mapping

Apply these scope rules:
- for this workspace, if the source is not stated, assume the user is giving HKJC prices rather than external prices
- when the user gives only HKJC prices, actively seek an accessible external benchmark before forming a directional view
- if the user provides external odds, treat them as the main pricing signal unless explicitly told otherwise
- if the user provides multiple external sources, prioritize sharp or consensus pricing over isolated soft-book prices
- if team names, competition names, kickoff context, or matchup identity are ambiguous, stop and ask the user to confirm the intended match before continuing
- use HKJC only after the external direction is formed; HKJC may improve or worsen execution quality, but should not become the core reason for a bet by itself
- if the user provides only non-HKJC prices, continue normally and label the result as external-market analysis
- if the user provides only HKJC prices and an external benchmark is found, label the result as HKJC input plus external benchmark analysis
- if the user provides only HKJC prices and no external benchmark can be obtained, analysis is still allowed, but clearly mark the view as incomplete value screening rather than a fully benchmarked directional read
- when market naming is ambiguous, keep the recommendation anchored to the handicap or total-goals context already provided by the user
- when any external public benchmark is used, identify the exact source in the response instead of referring vaguely to `external market`
- if no external public source was successfully checked, say so plainly and downgrade to incomplete screening or `PASS`

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

If external benchmark direct win odds are missing, do not invent them and do not silently replace them with HKJC alone. Continue only if the handicap-based rules are still usable, and explicitly state that the `Anti-Low-Value Rule` was not fully checked and that the result is only a partial value screen.

If external benchmark prices are referenced, they must be tied to named public sources actually checked during the analysis.

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
- if the team names or matchup mapping are unclear, ask the user to confirm the exact teams or competition instead of guessing
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
1. Restate the original decision: `PASS` or `PLAY`
2. Record the actual market outcome against the original quoted line
3. Check whether the original reasoning matched the intended rule branch
4. Separate execution error from strategy error
5. Adjust the strategy only if a repeatable rule weakness is visible across multiple results

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

Valid strategy-adjustment signals include:
- repeated failure of the same rule branch under similar season-phase conditions
- recurring misread of motivation or lifecycle mismatch inputs
- consistent evidence that a line type or source hierarchy is being interpreted incorrectly

If the new result is only one isolated sample, keep the strategy unchanged and log it as observation only.

## Decision Workflow

### 1. Start From a Defensive Default

Begin with the assumption that the match is a `PASS`.

Only move away from `PASS` if the matchup survives every filter below.

Only recommend `PLAY` when the setup supports both capital protection and a realistic stable-profit edge.

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

For a parlay:
- prefer `+0.5`
- do not use `+1` in parlays when a push would reduce that leg to `1.00`
- avoid void dilution that weakens real parlay progression

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

- balanced or coin-flip matches where both teams are in similar cycle state and no side has a strong structural edge

## Decision Priorities

When rules conflict, use this priority order:
1. `FORCE PASS` rules
2. market blacklist rules
3. cup first-leg and logistics friction warnings
4. motivation distortion warnings
5. underdog handicap protection rules
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
Assessment: Strategy Held | Execution Error | Review Needed
Sample Status: Observation | Watchlist | Formal Review | Eligible for Rule Change

What Was Correct:
- [rule or read that still holds]

What Was Wrong:
- [specific input read or rule application issue]

Adjustment Decision:
- Keep strategy unchanged | Watch for repeat pattern | Adjust rule
```

## Quality Checks Before Finishing

Before returning a recommendation, verify that:
- the answer explicitly states `PASS` or `PLAY`
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
- O/U recommendations are made only when the total-goals environment is structurally predictable
- O/U recommendations are not based only on recent overs, recent unders, or surface scoring trends
- post-match reviews do not force a rule change from a single isolated result

## Response Style

Keep the analysis readable and operational:
- lead with the external market read when an external benchmark is available
- mention HKJC mainly to explain whether the user-posted HKJC price is good enough to invest in relative to the external benchmark
- explain why a bet does or does not meet the stable-profit standard
- explain why the strategy prefers safety over payout
- if discussing O/U, separate structural tempo reasons from simple recent-score trends
- avoid giving multiple conflicting bet options
- if the edge is unclear, end with `PASS`