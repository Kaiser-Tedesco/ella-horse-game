# 🐎 Gallop & Friends — Handover Notes

Handover for a fresh session. This is Ella's horse website + learning games. One self-contained `index.html` (HTML + CSS + JS, no build step).

## Current status (2026-07-04)
- **Pushed 2026-07-04** (commit `0ea6e3b`), but the GitHub **Pages *deploy* step failed transiently** ("Deployment failed, try again later" — build succeeded, so content is fine). **The live site is NOT yet updated.** Fix: Actions tab → the failed run → **Re-run failed jobs** (or push any trivial commit). Confirm the live site actually shows the changes before assuming it's live.
- **Shipped this session (all verified in the preview, 0 console errors):**
  1. Renamed Heidi→**Margo**, Rani→**Rosie** (display names; image filenames unchanged). [Wölkchen was renamed Laeticia then reverted back to Wölkchen.]
  2. Vet content: +15 refresh cases, then the **year update** — sessions went 4→**5 questions**, Q1 always `longmult`, a **13-theme cycle**, **7 new maths builders**, **Sachaufgaben + free Hilfe** mode, +24 year-update cases. Concepts total **136** (1:1 with `EXAM`).
  3. **Cross-device save** (Firebase Realtime DB + manual backup). `FIREBASE_CONFIG` is still `null`, so cloud sync is **OFF** until Andrew does the ~15-min Firebase setup (steps in the Projects draft); the manual backup works now.
  4. **Bug fixes:** mode-aware `parseNum`; perfect-bonus now breaks on every wrong answer; convert answers ≥1000 accept Tausenderpunkte.
  5. **Mal-Quiz reward 5→3**; **+10 ranks (16-25)**; **+10 reward horses** (Merlin…Taki) with real Wikimedia photos.
- **Open items / optional tweaks (none blocking):** swap the Dülmener (herd shot) + Knabstrupper (two people in frame) photos; decide if Überschlagen should also accept the exact product (it currently only accepts the rounded estimate); Domino's rounding case can show ~990k circus spectators and Blanca's ×-tens case a "1000 g tube" (cartoonish scale, accepted for now).
- **Working docs** (in `C:\Users\User\Documents\Claude\Projects\ella-horse-game\`): `phase_log.md`, `plan_vetmaths_year_update.md`, `BACKUP_index_2026-07-04.html`, and DRAFTs `vet-refresh-DRAFT.md`, `vet-year-cases-DRAFT.md`, `phase3-firebase-sync-DRAFT.md`, `new-reward-horses-DRAFT.md`.

## Who it's for
- **Ella**, 9 years old, loves horses (especially small horses and ponies). German-speaking, also doing English at school. ADHD/dreamer attention profile, so the game leans on novelty, collecting, immediate feedback, and gentle (never punishing) mechanics.
- Tone for her: fun, friendly, encouraging, emoji-rich, simple. She uses voice input (expect spelling variations).
- **Maths focus (in the game, since the 2026-07 year update the FULL Grade-4 year):** written ×/+/− (with carrying/borrowing), big numbers to 1 000 000 (rounding), ×÷ by 10/100/1000, halbschriftlich, Überschlagen, money/decimals, unit-price compare, unit conversion (length/weight/time/volume incl. l/ml), time arithmetic, division-with-remainder, times-table recall (Blitz-Karten). Everything is presented as a **Sachaufgabe** (she picks the Rechen-Art herself; free Hilfe button reveals the set-up). German number format (comma decimal, € after the number, Tausenderpunkt accepted in input, e.g. `5,60 €`, `50.000`).
- A separate **summer practice topic list** for Ella's teacher was drafted (Sommer-Übungsplan, German, includes written +/−, große Zahlen, ÷ by 10/100/1000, etc.). That list is broader than what the game currently drills; the GAME has not added written addition/subtraction.

## Where everything lives
- **Local working folder (the ONLY source of truth):** `C:\Users\User\Documents\GitHub\ella-horse-game`
  - The old `OneDrive\...\Ella Website` folder was **deleted** — do not look for it.
- **Main file:** `index.html`.
- **Assets:** `horse-images/` (**35 photos**), `horse-sounds/` (3 mp3s). Keep these with `index.html`.
- **Also in folder:** `README.md`, `.gitignore`, two `vet_cases_*_print.md` (old English source notes, not used at runtime, not kept current).
- **Plan/status doc (outside the repo):** `C:\Users\User\Documents\Claude\Projects\ella-horse-game\FEATURE-PLAN.md` tracks the multi-phase build.
- **GitHub repo:** `kaiser-tedesco/ella-horse-game` · **Live:** https://kaiser-tedesco.github.io/ella-horse-game/

## How to deploy (GitHub Pages)
User pushes via **GitHub Desktop**: review changed files → Summary → **Commit to main** → **Push origin** (commit ≠ push; both needed). Wait ~1–2 min for the Pages build, then **hard-refresh** (Ctrl+F5) or incognito. "Not updating" is usually the Pages build still running (check the **Actions** tab) or browser cache. Assistant cannot `git push` from here (interactive auth fails).

## How to run/test locally
- Simplest: double-click `index.html`.
- Preview harness: `C:\Users\User\.claude\launch.json` (config name **gallop**, port 8099) + `static-server.ps1`. Use `preview_start` (name `gallop`), then drive with `preview_eval` against `http://localhost:8099/`.
- **Quirks:** `preview_screenshot` is unreliable here (times out) — verify via `preview_eval` reading DOM/state. The preview **reloads the page between separate eval calls**, so any multi-step DOM test (e.g. playing through the exam) must run inside ONE `preview_eval`. Top-level `const`/`let` (e.g. `horses`, `vetConcepts`, `EXAM`) are NOT on `window`; top-level `function` declarations ARE callable from eval.

## The site (features)
Single page that shows/hides "screens" via a `hidden` class.

1. **Main page** — rainbow header, **35 horse cards** (locked reward horses show as greyed "???" silhouettes), persistent **vet-rank card** + **🎓 exam button** when an exam is available, a **🎁 Meine Sammlung** collectibles album, the game buttons, and a German **"Welche Rechen-Arten"** explainer section above the footer.
2. **🎮 Horse Game** — see a clue, drag the right horse into the drop zone. Uses each horse's `questions` (5 each). **Only unlocked horses** appear (as the answer and as wrong choices).
3. **🩺 Vet Maths Game** — the main learning game (details below).
4. **🥕 Feed Kitchen** — just-for-fun, gated by "feed plays".
5. **⚡ Mal-Quiz: Blitz-Karten** — times-table practice.
6. **🔒 Grown-ups panel** — password **0101** → set vet points / feed plays, or reset to 0. Setting points here also sets the rank to match (no exam needed) and re-renders the grid.

## Vet Maths Game — how it works
- A session = **5 questions** (year update 2026-07). **Q1 is ALWAYS `longmult`** (schriftliche Multiplikation 3×2-stellig). Q2–Q5 walk a fixed **13-theme cycle** (`TOPIC_CYCLE`: add, wt, div, money, sub, len, tens, compare, round, zeit, halb, vol, estimate) via a persistent pointer (`ellaTopicCycle`, +4 per session, so full coverage every ~3¼ sessions); display order of the 4 shuffled. Theme→concept matching in `themeMatches()` (convert families read from the make() source via `conceptFam()`). Different horses per question where possible (curriculum beats horse variety on fallback).
- **Sachaufgaben mode:** every question starts as story-only (no operation shown, label says "Sachaufgabe"). A free **🖐 Hilfe button** (`revealHelp()`, `#mHelpBtn`) reveals `q.label` + `q.problemHTML`; the first wrong answer auto-reveals. Help costs nothing; only wrong answers break the perfect bonus. Multi-phase kinds (div step 2, compare pick) set `mHelpShown=true` so Hilfe can't overwrite their step labels.
- **Choose-your-patient:** the **first** of the 4 is hers to choose — "🐴 Welches Pferd braucht dich?" offers 2-3 candidate horses (same maths type, distinct from the other 3). The other 3 are system-picked.
- **All German.** Each question = a vet concept (with a sound-out aid for the technical term, e.g. *Hufabszess (Huf-aps-tsess)*) + a matching maths task.
- **12 skill types:** `longmult` (3×2 digit), `money` (€ × single digit), `compare` (two shops, unit price then dearer), `convert` (fams len/wt/dist/time/**vol**), `div` (estimate first, then exact **with remainder**), plus year-update types `add` (4-digit, carry forced), `sub` (borrow forced), `round` (round to Zehner/Hunderter/Tausender/Zehntausender; the number is sized to the place — ~0.6–9.6× — so it always rounds to a clean 1–10× that place, never 0 and never a place bigger than the number), `tens` (×÷10/100/1000, mode fixed per concept), `halb` (1×3-digit, Hilfe shows the Zerlegung), `estimate` (canonical round-to-hundreds product), `timecalc` (h min + min → minutes). Builders `Qadd/Qsub/Qround/Qtens/Qhalb/Qestimate/Qtimecalc` sit right above `vetConcepts`; story-arg conventions documented in the comment there.
- **`parseNum(v, dec)` is mode-aware** (do NOT revert it to stripping all dots — that broke dot-decimals like `0.054`): if a comma is present → comma is the decimal, dots are Tausenderpunkte; else for a **whole-number** answer (`dec` falsy) any dot is a Tausenderpunkt (`"50.000"`→50000); for a **decimal** answer (`dec` true) a lone dot **or** comma is the decimal point (`"0.054"` and `"0,054"`→0.054). `checkExactNum` gets `dec`; the **convert** case passes `!Number.isInteger(q.answer)` so whole ml/g/m answers accept Tausenderpunkte while kg/l/km decimals parse as decimals. Compared with `near()` for decimals, `===` for whole numbers.
- **Treatments (medicine step):** matched via the concept's `treatment` tag — `injection`💉 / `bucket`🪣 / `salve`🧴 (liquid fill), `bandage`🩹 (solid stack). Then drag the item onto the horse → "Geheilt!" cure card.
- **Collectible drop:** ~1-in-4 per heal, shown on the cure card (see Collectibles).
- **Focus helpers** (toggle 🔔, on by default, key `ellaNudgeOn`): a gentle idle sound after 45s of no input (rotates `sndNudge1/2/3`, currently the 3 cure sounds at low volume — PLACEHOLDER, swap in real soft nickers) + a 10-minute **hourglass** that refills with a soft synth chime at 0.
- **Session reward:** 10 points for finishing (15 if perfect first-try), +2 feed plays.

## Tierarzt-Prüfung (the exam) — gates level-ups
- Earning points to the next rank threshold **unlocks an exam** instead of auto-promoting. **Passing (80% / "Note 2,0") promotes her.** Failing is gentle: review the missed ones, retake, **points never lost**.
- Questions are **vet-concept comprehension** (e.g. "Was ist ein Hufabszess?"), 4-5 per exam, **drawn only from concepts she has actually seen** (tracked in `ellaSeenConcepts` as she plays).
- `EXAM` object is keyed by **concept title**; every vetConcept has exactly one entry (coverage is 1:1 — keep it that way when adding concepts).
- Pass screen shows a diploma and, if the new rank unlocks a reward horse, "🎉 Neues Pferd freigeschaltet!".

## Ranks & reward horses
- **26 ranks** (`VET_TIERS`, every 30 pts): 🌱Apprentice(0) 🐴Junior(30) 🩺Full(60) ⭐Senior(90) 🌟Super(120) 🦸Super Hero(150) 💫Mega(180) 🚀Ultra(210) 🏆Legendary(240) 📖Herriot(270) 🐵Goodall(300) 🌍Attenborough(330) 🐊Irwin(360) 🦍Fossey(390) 🐧Durrell(420) 🐢Darwin(450) 🐬Cousteau(480) 🦋Carson(510) 🌲Muir(540) 🐛Wallace(570) 🗺️Humboldt(600) 🦓Grzimek(630) 🦁Adamson(660) 🦅Audubon(690) 🦴Leakey(720) 🌸Linnaeus(750). (Ranks 9-25 named after vets/naturalists; ranks 16-25 added 2026-07.)
- **Rank model:** `getRankAchieved()` (key `ellaRankAchieved`); effective rank = `min(pointsTier, rankAchieved)`. **First run grandfathers** rankAchieved to `pointsTier − 1` (so the level her points just reached needs a test). **Grown-ups panel** sets it to the exact pointsTier (no test). `isHorseUnlocked(h)` = original (no `unlockRank`) OR `h.gift` OR `unlockRank ≤ rankAchieved`.
- **Reward horses unlock one per rank 6-25:** Pebbles(6/Mega), Mausi(7), Wölkchen(8), Keks(9), Flöckchen(10), Margo(11), Bonbon(12), Luzie(13), Pixie(14), Rosie(15/Darwin), then the 2026-07 additions: Merlin/Welsh Cob(16), Wicky/Dülmener(17), Perle/Caspian(18), Pixel/Pottok(19), Koni/Konik(20), Blanca/Camargue(21), Estrella/Lusitano(22), Balu/Shire(23), Domino/Knabstrupper(24), **Taki/Przewalski(25/Linnaeus)**. **Ottilia** is a **gift** (`gift:true`, unlocked from the start; she was Ella's "reached a level" present).
  - The 10 new horses (indices 25-34) were sourced with real Wikimedia photos (in `horse-images/`, breed-slug filenames). Their vet cases were appended under `// ===== NEW REWARD HORSES` in `vetConcepts`/`EXAM`. **Flag for a future pass:** the Dülmener photo is a herd (with foal) not a single portrait, and the Knabstrupper photo has two people in the background — swap if desired.
  - **Renamed 2026-07** (display name only): Heidi→**Margo**, Rani→**Rosie**. Their **image filenames were left unchanged** (`heidi-fell.jpg`, `rani-marwari.jpg`), so the file stems don't match the display names. Concept titles + `EXAM` keys moved with them (still 1:1). (Wölkchen/idx 17 was briefly renamed Laeticia this session then reverted; its `woelkchen-connemara.jpg` matches.)

## Collectibles & gifting
- Items come in **sets** (`COLLECTIBLE_SETS`, currently Set 1 + Set 2 of 9 hand-drawn inline-SVG items each; extensible — append more sets anytime). Lookup: `COLLECTIBLE_BY_ID`.
- **Drop:** ~1-in-4 per heal from the **current set** (`maybeDropCollectible`). Completing a set moves its items to a **gift tray** and starts the next set. Keys: `ellaCollectSet`, `ellaCollectProgress`, `ellaGifts`.
- **Gifting:** she drags a gift from the album onto a **horse card** (or tap-the-gift-then-tap-the-horse). It's **permanent**, removed from her tray, and shown under **"Geschenke von Ella"** in that horse's info modal. Key: `ellaHorseGifts` (`{horseIdx:[ids]}`).

## Flash cards (Mal-Quiz)
- 12 cards/round, **always includes 4×7 and 4×8**; rest random tables 2–9. Finishing → **+3 vet points, +1 feed play** (reduced from +5 on 2026-07 to slow horse-unlocking). No gate.

## Data model (in `index.html` `<script>`)
- `horses[]` — **35** objects: `name, breed, emoji, image, cardGradient, coat, temperament, story, facts[], questions[]`, plus reward horses have `unlockRank` (and Ottilia has `gift:true`). Indices 0-13 are the always-unlocked set (0 Stella … 11 Freya, **12 Chestnut/Suffolk Punch, 13 Saga/Icelandic**); 14 Ottilia (gift) … 24 Rosie; **25-34 = the 2026-07 additions (Merlin … Taki), unlockRank 16-25**.
- `vetConcepts[]` — **136** concepts (77 original + 15 refresh + 24 year-update + 20 for the new reward horses under `// ===== NEW REWARD HORSES`, 2 per horse) `{ horseIdx, type, treatment, title, intro, cured, make }`; `make()` calls a builder (`Qlongmult/Qmoney/Qdiv/Qconvert/Qcompare/Qadd/Qsub/Qround/Qtens/Qhalb/Qestimate/Qtimecalc`). **Every horse has ≥2 concepts.**
- `EXAM{}` — one MC entry per concept `{ q, correct, wrong:[2] }`, keyed by concept **title**.
- `VET_TIERS[]`, `COLLECTIBLE_SETS[]`, `TREATMENTS{}`, `buildMathPool()`, `renderHorseGrid()`, `isHorseUnlocked()`.
- **To add a reward horse:** append to `horses[]` with `unlockRank`; add ≥2 `vetConcepts` (`horseIdx` = its index); add a matching `EXAM` entry per concept title; source + resize a photo (below).

## localStorage keys (per device + per browser — NOT in the files)
`ellaVetPoints`, `ellaFeedTokens`, `ellaRankAchieved`, `ellaSeenConcepts`, `ellaNudgeOn`, `ellaCollectSet`, `ellaCollectProgress`, `ellaGifts`, `ellaHorseGifts`, `ellaTopicCycle` (year-update theme-cycle pointer, 0–12). Reset/set via Grown-ups panel (0101) or console.
- **Cross-device save (2026-07):** a self-contained module at the end of the main `<script>` monkey-patches `localStorage.setItem` to mirror every `ella*` progress key (except its own `ellaMagicWord` / `ellaSyncTs`) to a **Firebase Realtime Database** node `saves/<magic-word>`. Set the magic word once per computer in the Grown-ups panel (0101 → "☁️ Save across computers"); opening pulls the newer copy (timestamp-guarded), changes push back (~0.6 s debounce). **Not active until `FIREBASE_CONFIG` (currently `null`) is filled in** — see full setup steps + security rules in `Documents\Claude\Projects\ella-horse-game\phase3-firebase-sync-DRAFT.md`. Firebase SDK loaded via two compat `<script>` tags before `</head>`. A **manual backup** (copy/restore a base64 code) in the same panel works with no config and no internet.

## Assets / photos
- Horse photos from **Wikimedia Commons** (free/CC or PD). Pick clear, riderless, single-horse shots; download with a descriptive User-Agent; **resize to ~1000px long edge, quality ~82** (~80–250 KB). Resize via PowerShell `System.Drawing` (no ImageMagick installed). Wikimedia rate-limits bursts (HTTP 429) — download steadily.
- Sounds: 3 CC0 cure sounds (whinny/snort/stamp, bigsoundbank.com); the idle nudge reuses them quietly via `sndNudge1/2/3` (placeholder).

## Gotchas for next session
- Edit **`index.html` in the GitHub folder** only; user commits+pushes from GitHub Desktop. File is large, full of German + emoji (UTF-8) — prefer the Edit tool.
- **`EXAM` coverage must stay 1:1 with concept titles** — a missing/typo'd key means that concept can't be examined. Verify after adding concepts.
- **Locked reward horses are excluded** from the grid (silhouette), the vet maths pool, choose-your-patient, the guess-the-horse choices, and (via seen-tracking) the exam.
- `preview_screenshot` unreliable; page reloads between eval calls. Cross-eval `localStorage` also does **not** persist — set-then-check must be in ONE eval.
- **Perfect-session bonus:** `mPerfect` (init/reset true, read only in `showMathEnd`) must be set `false` on EVERY wrong-answer path. When adding a multi-phase question type, clear it in each wrong branch — the compare price/pick and div-estimate branches were leaking the bonus (fixed 2026-07). `mFirstTry` is dead code (assigned, never read).
- **When adding vet cases via an agent/workflow:** it sometimes returns `makeArrow` as the *full* builder call (`Qround((n,p)=>…)`) instead of just the arrow, which then gets double-wrapped into `Qround(Qround(…))` and crashes that question. Verify each `make()` actually runs after merging (loop `c.make()` in a preview eval).

## Possible next steps Ella might ask for
- More collectible sets; bigger unlock celebrations.
- Real soft-nicker sounds for the idle nudge (swap `sndNudge1/2/3`).
- Add written addition/subtraction and other Sommer-Übungsplan topics to the maths game.
- A grown-up stats/progress view. (Cross-device save was added 2026-07 — see the localStorage keys section.)
