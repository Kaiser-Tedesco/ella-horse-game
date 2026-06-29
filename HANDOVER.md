# 🐎 Gallop & Friends — Handover Notes

Handover for a fresh session. This is Ella's horse website + learning games. One self-contained `index.html` (HTML + CSS + JS, no build step).

## Who it's for
- **Ella**, 9 years old, loves horses (especially small horses and ponies). German-speaking, also doing English at school. ADHD/dreamer attention profile, so the game leans on novelty, collecting, immediate feedback, and gentle (never punishing) mechanics.
- Tone for her: fun, friendly, encouraging, emoji-rich, simple. She uses voice input (expect spelling variations).
- **Maths focus (in the game):** long multiplication with carrying, money/decimals, comparing unit prices, unit conversion (length/weight/time/volume), division-with-remainder, estimation/rounding, times-table recall. German number format (comma decimal, € after the number, e.g. `5,60 €`, `3,5 km`).
- A separate **summer practice topic list** for Ella's teacher was drafted (Sommer-Übungsplan, German, includes written +/−, große Zahlen, ÷ by 10/100/1000, etc.). That list is broader than what the game currently drills; the GAME has not added written addition/subtraction.

## Where everything lives
- **Local working folder (the ONLY source of truth):** `C:\Users\User\Documents\GitHub\ella-horse-game`
  - The old `OneDrive\...\Ella Website` folder was **deleted** — do not look for it.
- **Main file:** `index.html`.
- **Assets:** `horse-images/` (**25 photos**), `horse-sounds/` (3 mp3s). Keep these with `index.html`.
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

1. **Main page** — rainbow header, **25 horse cards** (locked reward horses show as greyed "???" silhouettes), persistent **vet-rank card** + **🎓 exam button** when an exam is available, a **🎁 Meine Sammlung** collectibles album, the game buttons, and a German **"Welche Rechen-Arten"** explainer section above the footer.
2. **🎮 Horse Game** — see a clue, drag the right horse into the drop zone. Uses each horse's `questions` (5 each). **Only unlocked horses** appear (as the answer and as wrong choices).
3. **🩺 Vet Maths Game** — the main learning game (details below).
4. **🥕 Feed Kitchen** — just-for-fun, gated by "feed plays".
5. **⚡ Mal-Quiz: Blitz-Karten** — times-table practice.
6. **🔒 Grown-ups panel** — password **0101** → set vet points / feed plays, or reset to 0. Setting points here also sets the rank to match (no exam needed) and re-renders the grid.

## Vet Maths Game — how it works
- A session = **4 questions**, weighted across skills, div appearing occasionally (~40%, at most once). Always 4 **different unlocked horses**.
- **Choose-your-patient:** the **first** of the 4 is hers to choose — "🐴 Welches Pferd braucht dich?" offers 2-3 candidate horses (same maths type, distinct from the other 3). The other 3 are system-picked.
- **All German.** Each question = a vet concept (with a sound-out aid for the technical term, e.g. *Hufabszess (Huf-aps-tsess)*) + a matching maths task.
- **5 skill types:** `longmult` (3×2 digit, carry reminder), `money` (€ × single digit), `compare` (two shops, unit price then dearer), `convert` (km/m/cm, kg/g, hours→min, l/ml), `div` (estimate first, then exact **with remainder**).
- Inputs accept a **comma decimal** (`parseNum`, compared with `near()`).
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
- **16 ranks** (`VET_TIERS`, every 30 pts): 🌱Apprentice(0) 🐴Junior(30) 🩺Full(60) ⭐Senior(90) 🌟Super(120) 🦸Super Hero(150) 💫Mega(180) 🚀Ultra(210) 🏆Legendary(240) 📖Herriot(270) 🐵Goodall(300) 🌍Attenborough(330) 🐊Irwin(360) 🦍Fossey(390) 🐧Durrell(420) 🐢Darwin(450). (Ranks 9-15 named after vets/naturalists.)
- **Rank model:** `getRankAchieved()` (key `ellaRankAchieved`); effective rank = `min(pointsTier, rankAchieved)`. **First run grandfathers** rankAchieved to `pointsTier − 1` (so the level her points just reached needs a test). **Grown-ups panel** sets it to the exact pointsTier (no test). `isHorseUnlocked(h)` = original (no `unlockRank`) OR `h.gift` OR `unlockRank ≤ rankAchieved`.
- **Reward horses unlock one per rank 6-15:** Pebbles(6/Mega), Mausi(7), Wölkchen(8), Keks(9), Flöckchen(10), Heidi(11), Bonbon(12), Luzie(13), Pixie(14), **Rani(15/Darwin)**. **Ottilia** is a **gift** (`gift:true`, unlocked from the start; she was Ella's "reached a level" present).

## Collectibles & gifting
- Items come in **sets** (`COLLECTIBLE_SETS`, currently Set 1 + Set 2 of 9 hand-drawn inline-SVG items each; extensible — append more sets anytime). Lookup: `COLLECTIBLE_BY_ID`.
- **Drop:** ~1-in-4 per heal from the **current set** (`maybeDropCollectible`). Completing a set moves its items to a **gift tray** and starts the next set. Keys: `ellaCollectSet`, `ellaCollectProgress`, `ellaGifts`.
- **Gifting:** she drags a gift from the album onto a **horse card** (or tap-the-gift-then-tap-the-horse). It's **permanent**, removed from her tray, and shown under **"Geschenke von Ella"** in that horse's info modal. Key: `ellaHorseGifts` (`{horseIdx:[ids]}`).

## Flash cards (Mal-Quiz)
- 12 cards/round, **always includes 4×7 and 4×8**; rest random tables 2–9. Finishing → **+5 vet points, +1 feed play**. No gate.

## Data model (in `index.html` `<script>`)
- `horses[]` — **25** objects: `name, breed, emoji, image, cardGradient, coat, temperament, story, facts[], questions[]`, plus reward horses have `unlockRank` (and Ottilia has `gift:true`). Indices 0-13 are the always-unlocked set (0 Stella … 11 Freya, **12 Chestnut/Suffolk Punch, 13 Saga/Icelandic**); 14 Ottilia (gift) … 24 Rani.
- `vetConcepts[]` — **~77** concepts `{ horseIdx, type, treatment, title, intro, cured, make }`; `make()` calls a builder (`Qlongmult/Qmoney/Qdiv/Qconvert/Qcompare`). **Every horse has ≥2 concepts.**
- `EXAM{}` — one MC entry per concept `{ q, correct, wrong:[2] }`, keyed by concept **title**.
- `VET_TIERS[]`, `COLLECTIBLE_SETS[]`, `TREATMENTS{}`, `buildMathPool()`, `renderHorseGrid()`, `isHorseUnlocked()`.
- **To add a reward horse:** append to `horses[]` with `unlockRank`; add ≥2 `vetConcepts` (`horseIdx` = its index); add a matching `EXAM` entry per concept title; source + resize a photo (below).

## localStorage keys (per device + per browser — NOT in the files)
`ellaVetPoints`, `ellaFeedTokens`, `ellaRankAchieved`, `ellaSeenConcepts`, `ellaNudgeOn`, `ellaCollectSet`, `ellaCollectProgress`, `ellaGifts`, `ellaHorseGifts`. Reset/set via Grown-ups panel (0101) or console.

## Assets / photos
- Horse photos from **Wikimedia Commons** (free/CC or PD). Pick clear, riderless, single-horse shots; download with a descriptive User-Agent; **resize to ~1000px long edge, quality ~82** (~80–250 KB). Resize via PowerShell `System.Drawing` (no ImageMagick installed). Wikimedia rate-limits bursts (HTTP 429) — download steadily.
- Sounds: 3 CC0 cure sounds (whinny/snort/stamp, bigsoundbank.com); the idle nudge reuses them quietly via `sndNudge1/2/3` (placeholder).

## Gotchas for next session
- Edit **`index.html` in the GitHub folder** only; user commits+pushes from GitHub Desktop. File is large, full of German + emoji (UTF-8) — prefer the Edit tool.
- **`EXAM` coverage must stay 1:1 with concept titles** — a missing/typo'd key means that concept can't be examined. Verify after adding concepts.
- **Locked reward horses are excluded** from the grid (silhouette), the vet maths pool, choose-your-patient, the guess-the-horse choices, and (via seen-tracking) the exam.
- `preview_screenshot` unreliable; page reloads between eval calls.

## Possible next steps Ella might ask for
- More collectible sets; bigger unlock celebrations.
- Real soft-nicker sounds for the idle nudge (swap `sndNudge1/2/3`).
- Add written addition/subtraction and other Sommer-Übungsplan topics to the maths game.
- A grown-up stats/progress view; optional cross-device save (needs a server — big job).
