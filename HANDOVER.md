# 🐎 Gallop & Friends — Handover Notes

Handover for a fresh session. This is Ella's horse website + learning games. One self-contained file.

## Who it's for
- **Ella**, 9 years old, loves horses. German-speaking, also doing English at school.
- Tone for her: fun, friendly, encouraging, emoji-rich, simple. She uses voice input (expect spelling variations).
- **Maths focus (current):** long multiplication with carrying, money/decimals, unit conversion, division-with-remainder, estimation/rounding, and times-table recall. Decimals are now in use; number format is German (comma decimal, € after the number, e.g. `5,60 €`, `3,5 km`).

## Where everything lives
- **Local working folder (the ONLY source of truth):** `C:\Users\User\Documents\GitHub\ella-horse-game`
  - The old `OneDrive\...\Ella Website` folder was **deleted** — do not look for it.
- **Main file:** `index.html` (everything is in here: HTML + CSS + JS, no build step).
- **Assets:** `horse-images/` (12 photos), `horse-sounds/` (3 mp3s). Keep these with `index.html`.
- **Also in folder:** `README.md` (GitHub setup guide), `.gitignore`, two `vet_cases_*_print.md` (source notes for the vet concepts — not used at runtime).
- **GitHub repo:** `kaiser-tedesco/ella-horse-game`
- **Live site:** https://kaiser-tedesco.github.io/ella-horse-game/

## How to deploy (GitHub Pages)
The user pushes via **GitHub Desktop**: it shows changed files → type a Summary → **Commit to main** → **Push origin** (commit ≠ push; both are needed). Then wait ~1–2 min for the Pages build and **hard-refresh** (Ctrl+F5) or use an incognito window.
- "Not updating" is almost always (a) Pages build still *in progress* — check the repo's **Actions** tab for a green tick — or (b) browser cache. The push itself is usually fine.
- Assistant cannot `git push` from here (interactive auth fails); the user pushes from Desktop.

## How to run/test locally
- Simplest: double-click `index.html`.
- Assistant preview harness is already set up: `C:\Users\User\.claude\launch.json` (config name **gallop**, port 8099) + `C:\Users\User\.claude\static-server.ps1` (a tiny PowerShell static server, `-Root` points at the GitHub folder). Use `preview_start` (name `gallop`) → drive with `preview_eval` against `http://localhost:8099/`.
- **Quirk:** `preview_screenshot` is flaky in this environment (often times out / reverts to main page). Verify via `preview_eval` (read DOM/state) instead — it's reliable. Navigate with `location.href='http://localhost:8099/'` after start.

## The site (features)
Single page that shows/hides "screens" via a `hidden` class.

1. **Main page** — rainbow header, **12 horse cards** (click → modal with photo, coat, temperament, backstory, fun facts), a persistent **vet-rank card**, and the game buttons.
2. **🎮 Horse Game** — see a clue, drag the right horse photo into the drop zone. Uses each horse's `questions` array (5 each). All 12 horses participate.
3. **🩺 Vet Maths Game** — the main learning game (details below).
4. **🥕 Feed Kitchen** — just-for-fun: pick 4 random horses, drag food into a bucket, fill the troughs. **Gated by "feed plays"** (earned by playing the maths/flash games).
5. **⚡ Mal-Quiz: Blitz-Karten (flash cards)** — times-table practice (details below).
6. **🔒 Grown-ups panel** — small link on main page; password **0101** → set vet points and feed plays, or reset to 0. (Light protection only; the password is in the page source.)

## Vet Maths Game — how it works
- A session = **4 questions**, weighted to the new skills, with division/rounding appearing occasionally (~40%, at most once). Always 4 **different horses**.
- **All German.** Each question = a short vet concept (with a sound-out aid for the technical term, e.g. *Hufabszess (Huf-aps-tsess)*) + a coherent maths task that follows from it.
- **5 skill types** (each concept is fixed to one, and its task is written to match):
  - `longmult` — 3-digit × 2-digit, shown in column format with a carry reminder.
  - `money` — €amount (2 decimals) × single digit.
  - `compare` — two shops; work out each unit price, then pick the dearer (two-phase).
  - `convert` — km↔m, m↔cm, kg↔g, and **hours→minutes only** (whole hours; no fractions yet).
  - `div` — division; **estimate first** (round → rough answer, "close enough" band), then exact answer **with remainder (Rest)**. Two-phase.
- Inputs accept a **comma decimal** (text inputs, parsed via `parseNum`). Decimals compared with `near()`.
- **Treatments (medicine-making step):** after a correct answer she makes the medicine, matched to the case via the concept's `treatment` tag:
  - `injection` 💉, `bucket` 🪣, `salve` 🧴 = liquid "fill the vessel" mini-game.
  - `bandage` 🩹 = **solid tray**, layers stack up (not liquid).
  - Then she drags the finished item onto the horse (themed splash), → "Geheilt!" cure card → next.

### Data model (in `index.html` `<script>`)
- `horses[]` — 12 horse objects: `name, breed, emoji, image, cardGradient, coat, temperament, story, facts[], questions[]`. (Indices: 0 Stella, 1 Biscuit, 2 Thunder, 3 Dotty, 4 Midnight, 5 Sunny, 6 Patches, 7 Cupcake, 8 Brio, 9 Luna, **10 Aurora (Akhal-Teke)**, **11 Freya (Norwegian Fjord)**.)
- `vetConcepts[]` — ~49 concepts: `{ horseIdx, type, treatment, title, intro, cured, make }`. `make()` calls a builder (`Qlongmult/Qmoney/Qdiv/Qconvert/Qcompare`) with a German story function; builders generate fresh numbers each play.
- `TREATMENTS{}` — the 4 medicine-making mini-games (supplies, vessel, colours, finished item, splash; bandage has `style:'stack'`).
- `buildMathPool()` — picks the 4 question types + distinct horses each session.
- **To add a concept:** add an object to `vetConcepts` with a `type`, a fitting `treatment`, and a `make:()=>Q…((...)=>` German task `)`. Aurora & Freya already have 4 each. (The two `vet_cases_*.md` files are the English source ideas.)

## Flash cards (Mal-Quiz)
- 12 cards/round. **Always includes 4×7 and 4×8** (her tricky ones); the rest are random hard tables **2–9 (no ×1, no ×10)**, shuffled.
- Type the answer; wrong → retry; finishing all 12 → **+5 vet points and +1 feed play**. German UI. No gate (this is how she earns).

## Points / ranks / feed economy (all persistent via browser `localStorage`)
- **Vet points** key `ellaVetPoints`; **feed plays** key `ellaFeedTokens`.
- **Vet Maths session reward:** **full 10 points for finishing** (she always eventually gets each right — retries don't cost her), **+5 bonus for a perfect first-try round (=15)**. Either way **+2 feed plays**.
- **Flash cards:** +5 points, +1 feed play per completed round.
- **Ranks (every 30 pts):** 🌱 Apprentice (0) → 🐴 Junior Vet (30) → 🩺 Full Vet (60) → ⭐ Senior Vet (90) → 🌟 Super Vet (120) → 🦸 Super Hero Vet (150). The main-page card nudges "Earn just X more points to become…".
- **Important:** progress is saved **per device + per browser**, NOT in the files — it does NOT travel with the repo or sync across devices. To set/reset a score: Grown-ups panel (0101), or browser console `localStorage.setItem('ellaVetPoints','90')`.

## Assets / photos
- All horse photos are from **Wikimedia Commons** (free/CC). When adding: pick a clear, riderless, single-horse color photo; download with a descriptive User-Agent; **resize to ~1000px long edge, quality ~82** (the others are ~120–220 KB). Wikimedia **rate-limits** bursts (HTTP 429) — download one at a time, wait if blocked.
- Recent additions: Aurora = Akhal-Teke "golden" photo (CC BY-SA 4.0, Artur Baboev); Freya = Norwegian Fjord (public domain, Plasma).
- Sounds: 3 horse sounds (whinny/snort/stamp), CC0 from bigsoundbank.com; a random one plays when a horse is healed.

## Gotchas for next session
- Edit **`index.html` in the GitHub folder** only. After editing, the user commits+pushes from GitHub Desktop.
- The file is large and full of German + emoji (UTF-8). Prefer the Edit tool. If scripting edits in PowerShell, read/write with **explicit UTF-8** (`[IO.File]::ReadAllText/WriteAllText` with `UTF8Encoding($false)`) to avoid mangling.
- `preview_screenshot` unreliable here — verify with `preview_eval`.
- Vet cases currently use horses 0–11; the `.md` source files weren't updated with Aurora/Freya cases (not needed).

## Possible next steps Ella might ask for
- Weave Aurora & Freya into more vet concepts; add more horses.
- A "stamp/snort" sound on each ingredient drop; bigger level-up celebration.
- Distinct injection vessel (syringe shape) instead of the bowl.
- A progress/stats view for grown-ups; optional cross-device save (needs a server — big job).
