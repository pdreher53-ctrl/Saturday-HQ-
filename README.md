# Handoff: Saturday HQ

Mobile-first college football companion app: **Command Center** (what to watch), **Chaos Index** (0–100 "how weird will this get" score), **Receipts** (friends lock predictions, evidence kept).

## About the Design Files
The files in `design-reference/` are **design references created in HTML** — high-fidelity interactive prototypes showing intended look and behavior, not production code to ship. The task is to **recreate these designs as a React + TypeScript app** (per the product brief: local state only, mock data in separate files, no backend/auth/external APIs — clean enough to deploy to Netlify and keep developing in VS Code). Open `Saturday HQ standalone.html` in any browser to see the working prototype; `Saturday HQ.dc.html` contains the readable source (template markup + a `Component` logic class with all mock data and state logic).

## Fidelity
**High-fidelity.** Colors, typography, spacing, copy, and interactions are final. Recreate pixel-perfectly.

## Design Tokens
Colors:
- Background `#0C0D11`, elevated card `#14161C`, inset panel `#0F1014`, ticker strip `#101218`
- Text: primary `#F3EFE7` (warm off-white), body `#C6CBD4`, secondary `#98A0AD` (slate), dim `#6B7280`, faint `#3A3F4B`
- Accents: gold `#F5A524` (primary/CTA), orange `#FF8A3D` (Maximum Chaos), red `#FF5C5C` (Drop Everything/alerts), green `#3DD68C` (success), muted gold `#D9B84A`
- Borders: `rgba(255,255,255,0.07)` default, `0.12` on chips/inputs
- Team colors: 44-team map in the logic class (`team()` method) — abbreviation + primary hex, text color auto-contrast (luma > 150 → dark text)

Typography:
- Display: **Barlow Condensed** 600/700/800 (+ italic 800 for wordmark/stamps) — all headings, scores, labels, badges
- Body: **Instrument Sans** 400–700
- Scale: h1 38px/800, card titles 19–28px/700 condensed, chaos scores 28–72px/800 condensed, body 13–15px, micro-labels 10–13px with 0.1–0.2em letter-spacing, uppercase

Shape & spacing: cards radius 14–16px, chips/pills 999px, buttons 10px; card padding 14–20px; section gaps 10–12px; content max-width **1160px** centered, 16px side padding.

## Screens

### 1. Command Center (default)
- Sticky header: italic wordmark `SATURDAY HQ` (HQ in gold), `WEEK 1` chip, avatar; nav row (Command Center / Chaos Index / Receipts, active = warm white + 2px gold underline).
- **Chaos Cam ticker**: thin strip under header, pulsing red dot + `CHAOS CAM` label, message rotates every 4s (5 messages in source).
- Title + "Your plan for college football today." + date line (Saturday, September 5 · 22 games · 72° and clear).
- **National Chaos Meter** (hero card, orange-tinted border): 72px `91`, `NATIONAL CHAOS` label, gold→red progress bar at 91%, "Today is a 91. Clear your schedule." Beside it: **Chaos Forecast** card, 3 bullets with gold `›`.
- Viewing-window pills (Noon/Afternoon/Primetime/Late Night) + right-aligned **SICKO MODE** toggle (green when on; re-sorts list by degeneracy score, header becomes "CERTIFIED SICKO BOARD — {WINDOW}").
- **Featured trio** (auto-fit grid ≥280px): PRIMARY GAME / SECOND SCREEN / CHAOS GAME cards — 4px split team-color bar on top, role label, condensed matchup, time·network, spread, chaos score in tier color, reason text. Chaos card gets warm gradient bg + orange border + `MAXIMUM CHAOS`.
- **All games list**: rows with stacked team-abbreviation badges (team-color bg), matchup + meta (time · TV · spread · O/U) + tag chips, right-aligned chaos score + tier label. 3px left border in tier color (≥50), subtle red/orange gradient tint for 95+/85+.
- **Chaos Alert card** (red-tinted, clickable, `OPEN GAME →`): opens full-screen **DROP EVERYTHING takeover** (radial dark-red bg, 140px `97`, score, `OPEN GAME` red button, tap-anywhere dismiss).
- **GROUP ACTIVITY**: avatar + "Jake locked Florida outright"-style in-app actions.
- **RECEIPTS preview**: 3 locked predictions + "View All Receipts" (navigates to Receipts).

### 2. Chaos Index
- Header + "How is Chaos calculated?" button → methodology modal (8 factors, entertainment-not-gambling disclaimer).
- Conference chips (All Games/Top 25/SEC/Big Ten/ACC/Big 12/Other) + window chips (All Day + 4).
- **THE CHAOS PODIUM** (gold heading): top 3 as expanded cards — rank, matchup, meta, big score + tier, 5 factor bars (Competitiveness, Upset Potential, Volatility, Rivalry/Emotion, Scoring Environment), `WHY IT'S CHAOTIC` bullets.
- **THE BOARD**: #4+ as compact ranked rows (same tier accents).

### 3. Receipts
- Header, "Talk is cheap. We keep the evidence.", gold `+ Make a Receipt` button, group switcher pill (Saturday Sickos).
- **Leaderboard**: rank, name + title chip (THE ORACLE gold / THE HEDGER slate / THE MILK MAN red), record, streak line, Receipt Rating (green ≥70, white ≥50, red below). #1 row gold-tinted.
- **THE SUNDAY PAPER** recap card: MOVER / BEST RECEIPT / WORST RECEIPT / CHAOS GAME rows + "Share the recap" button (fake copy, label swaps 2s).
- **RECEIPT FOUND** share card: dashed border, red stamp headline, quote, FINAL score, rotated `AGED LIKE MILK` stamp, share button.
- Receipt cards: user, status badge (LOCKED slate / NAILED IT green / AGED LIKE MILK red, etc.), quoted prediction, game · timestamp, result row when resolved.

### Game Detail (bottom sheet, all screens)
Matchup + split color bar, time/TV/spread/O-U, big chaos score panel, WHY WE'RE WATCHING bullets, CHAOS BREAKDOWN bars, GROUP CONSENSUS split bar (gold vs slate + "Fade the group and win: +10 Receipt Rating."), `ADD TO WATCH PLAN` (toggles to green ✓ state) + `MAKE A RECEIPT`.

### Make a Receipt (modal)
Textarea, game select, prediction-type chips (Winner/Spread/Player/Team/Hot Take/Custom), confidence 1–5, gold `LOCK RECEIPT`, "Once the game starts, your receipt cannot be edited." Locking adds a `You` receipt locally and navigates to Receipts.

## Chaos Tiers
95–100 DROP EVERYTHING `#FF5C5C` · 85–94 MAXIMUM CHAOS `#FF8A3D` · 70–84 CHAOS WATCH `#F5A524` · 50–69 GETTING WEIRD `#D9B84A` · 30–49 WORTH MONITORING `#98A0AD` · 0–29 PROBABLY NORMAL `#6B7280`

## State
`tab`, `win` (viewing window), `conf` + `cwin` (Chaos Index filters), `selId` (detail sheet), `method`/`rmodal`/`takeover` (modals), receipt form (`rtext`, `rgame`, `rtype`, `rconf`), `myReceipts[]`, `watch{}`, `sicko`, `tickerIdx` (4s interval), transient share-button labels. All local; mock data = 22 games with derived tier/breakdown/consensus (see `games()`, `deco()`, `tier()` in the logic class).

## Suggested Component Structure
AppShell, Header, Navigation, ChaosTicker, ChaosMeter, ViewingWindowTabs, FeaturedGameCard, GameCard, TeamBadge, ChaosBadge, ChaosBreakdown, GameDetail, MethodologyModal, FriendActivity, ReceiptPreview, ReceiptCard, ReceiptLeaderboard, ReceiptModal, ShareReceiptCard, SundayPaper, FilterBar, TakeoverAlert. Mock data in `data/games.ts`, `data/receipts.ts`, `data/teams.ts`.

## Assets
No image assets. Fonts from Google Fonts (Barlow Condensed, Instrument Sans). Team identity is abbreviations + hex colors only — **no school logos** (deliberate, for IP reasons).

## Creating the repo
```bash
git init saturday-hq && cd saturday-hq
# scaffold e.g.: npm create vite@latest . -- --template react-ts
# copy design_handoff_saturday_hq/ into the repo as design/
git add -A && git commit -m "Saturday HQ design handoff"
gh repo create saturday-hq --private --source=. --push
```
