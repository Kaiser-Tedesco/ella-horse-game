# 🐎 Gallop & Friends — Handover Notes

Handover for a fresh session. This is Ella's horse website + learning games. One self-contained `index.html` (HTML + CSS + JS, no build step).

## Current status (2026-07-07)
- **Deploy status (READ FIRST):** local `index.html` has **uncommitted changes** on top of the earlier backlog; the newest is the **trinket/collectibles revive** (Set 3 + refilling supply, see the 2026-07-07 note and the Collectibles section). **Next step: commit + push the current local `index.html` via GitHub Desktop**, then hard-refresh (Ctrl+F5) and confirm the live site updated. History: commit `0ea6e3b` once failed at the Pages *deploy* step, so a fresh commit is needed to re-trigger build+deploy and ship everything at once (a bare "re-run" of the old run only ships `0ea6e3b`).
- **Shipped 2026-07-07 (verified in preview, 0 console errors):**
  - **Revived the trinket/collectibles reward system.** Added **Set 3** ("Turnier & Stall": Pokal, Medaille, Blumenkranz, Glocke, Wassermelone, Trense, Mähnenbürste, Namensschild, Laterne), so the game now has **3 sets / 27 items**. Switched gifting to a **refilling supply**: every ~1-in-4 heal now also drops a giftable copy into her tray, and once all sets are discovered drops keep coming from the full pool, so it never dead-ends (the old model went dead once both sets were collected and all 18 given away). Cure-card wording switches between "Neu für deine Sammlung" and "Zum Verschenken bekommen". Full detail in **Collectibles & gifting** below.
  - **Confirmed cross-device save is now LIVE** (the earlier "off" note below was stale): `FIREBASE_CONFIG` is filled in (project `ella-horse-game`, Realtime DB region `europe-west1`), the Firebase SDK loads, and the DB is reachable (verified via a read probe, HTTP 200). The DB is intentionally left **open/flexible** (public read/write, no security-rules lockdown), an accepted low-stakes tradeoff for a child's game-progress save. Magic-word cloud sync and the manual base64 backup both work from the Grown-ups panel (0101).
- **Shipped 2026-07-04 (all verified in the preview, 0 console errors):**
  1. Renamed Heidi→**Margo**, Rani→**Rosie** (display names; image filenames unchanged). [Wölkchen was renamed Laeticia then reverted back to Wölkchen.]
  2. Vet content: +15 refresh cases, then the **year update** — sessions went 4→**5 questions**, Q1 always `longmult`, a **13-theme cycle**, **7 new maths builders**, **Sachaufgaben + free Hilfe** mode, +24 year-update cases. Concepts total **136** (1:1 with `EXAM`).
  3. **Cross-device save** (Firebase Realtime DB + manual backup) was built this session. [Update 2026-07-07: `FIREBASE_CONFIG` has since been filled in and cloud sync is **LIVE** — see the 2026-07-07 note above. The manual backup also works.]
  4. **Bug fixes:** mode-aware `parseNum`; perfect-bonus now breaks on every wrong answer; convert answers ≥1000 accept Tausenderpunkte; **`Qround` sizes the number to the place** (always rounds to a clean 1–10× the place, never 0, never a place bigger than the number — e.g. 830→Tausender=1000).
  5. **Mal-Quiz reward 5→3**; **+10 ranks (16-25)**; **+10 reward horses** (Merlin…Taki) with real Wikimedia photos.
- **Open items / optional tweaks (none blocking):** swap the Dülmener (herd shot) + Knabstrupper (two people in frame) photos; decide if Überschlagen should also accept the exact product (it currently only accepts the rounded estimate); Blanca's ×-tens case can show a "1000 g tube" (cartoonish scale, accepted for now).
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
- Items come in **sets** (`COLLECTIBLE_SETS`, currently **3 sets of 9** hand-drawn inline-SVG items each = **27 total**: Set 1 feed/tack basics, Set 2 grooming/care, Set 3 "Turnier & Stall"; extensible, append more sets anytime). Lookup: `COLLECTIBLE_BY_ID`.
- **Drop / refilling supply (reworked 2026-07-07):** ~1-in-4 per heal via `maybeDropCollectible`. The **album** (`ellaCollectProgress` + `ellaCollectSet`) is the "collect one of each" layer, filled set-by-set; on set completion the pointer advances to the next set. **Every drop also pushes a giftable copy into her tray** (`ellaGifts`), so she has trinkets to give from the very first find. **Once all sets are discovered, drops don't stop:** a random item from the full 27-pool drops into the tray, so the giveaway supply keeps refilling (only runs dry if she stops playing). The function returns `{id, name, svg, isNew}`; `isNew` selects the cure-card label ("Neu für deine Sammlung" vs "Zum Verschenken bekommen"). Note the album marks an item collected permanently, independent of whether its giftable copy has been given away. (Old model, pre-2026-07-07: giftables appeared only when a whole set completed, then the whole feature went dead once both sets were collected and all 18 given away.)
- **Gifting (consumptive):** she drags a gift from the album onto a **horse card** (or tap-the-gift-then-tap-the-horse). It's **permanent** on the horse and **removed from her tray** (the tray refills from later drops), shown under **"Geschenke von Ella"** in that horse's info modal. Key: `ellaHorseGifts` (`{horseIdx:[ids]}`). Possible future tidy-up: the tray grows one-per-drop and only shrinks on gifting, so a heavy-play/no-gift streak makes it long (flex-wraps fine); could stack duplicate tokens with a count if desired.

## Flash cards (Mal-Quiz)
- 12 cards/round, **always includes 4×7 and 4×8**; rest random tables 2–9. Finishing → **+3 vet points, +1 feed play** (reduced from +5 on 2026-07 to slow horse-unlocking). No gate.

## Data model (in `index.html` `<script>`)
- `horses[]` — **35** objects: `name, breed, emoji, image, cardGradient, coat, temperament, story, facts[], questions[]`, plus reward horses have `unlockRank` (and Ottilia has `gift:true`). Indices 0-13 are the always-unlocked set (0 Stella … 11 Freya, **12 Chestnut/Suffolk Punch, 13 Saga/Icelandic**); 14 Ottilia (gift) … 24 Rosie; **25-34 = the 2026-07 additions (Merlin … Taki), unlockRank 16-25**.
- `vetConcepts[]` — **136** concepts (77 original + 15 refresh + 24 year-update + 20 for the new reward horses under `// ===== NEW REWARD HORSES`, 2 per horse) `{ horseIdx, type, treatment, title, intro, cured, make }`; `make()` calls a builder (`Qlongmult/Qmoney/Qdiv/Qconvert/Qcompare/Qadd/Qsub/Qround/Qtens/Qhalb/Qestimate/Qtimecalc`). **Every horse has ≥2 concepts.**
- `EXAM{}` — one MC entry per concept `{ q, correct, wrong:[2] }`, keyed by concept **title**.
- `VET_TIERS[]`, `COLLECTIBLE_SETS[]`, `TREATMENTS{}`, `buildMathPool()`, `renderHorseGrid()`, `isHorseUnlocked()`.
- **To add a reward horse:** append to `horses[]` with `unlockRank`; add ≥2 `vetConcepts` (`horseIdx` = its index); add a matching `EXAM` entry per concept title; source + resize a photo (below).

## localStorage keys (per device + per browser — NOT in the files)
`ellaVetPoints`, `ellaFeedTokens`, `ellaRankAchieved`, `ellaSeenConcepts`, `ellaNudgeOn`, `ellaCollectSet`, `ellaCollectProgress`, `ellaGifts`, `ellaHorseGifts`, `ellaTopicCycle` (year-update theme-cycle pointer, 0–12). Plus three NON-progress keys that are **not** mirrored to the cloud (in the sync `SKIP` set): `ellaMagicWord` (active player = cloud key), `ellaSyncTs` (last-change timestamp), `ellaPlayers` (device-local player list for the picker). Reset/set via Grown-ups panel (0101) or console.
- **Cross-device save (2026-07, LIVE):** a self-contained module at the end of the main `<script>` monkey-patches `localStorage.setItem` to mirror every `ella*` progress key (except its own `ellaMagicWord` / `ellaSyncTs`) to a **Firebase Realtime Database** node `saves/<magic-word>`. Set the magic word once per computer in the Grown-ups panel (0101 → "☁️ Save across computers"); opening pulls the newer copy (timestamp-guarded), changes push back (~0.6 s debounce). The magic word IS the identity (no password), so anyone who knows it can load/overwrite that save. **`FIREBASE_CONFIG` is now filled in** (project `ella-horse-game`, RTDB region `europe-west1`) **and sync is active** (verified 2026-07-07: SDK loads, DB reachable, HTTP 200). The DB rules are **flexible but not wide open**: a specific `saves/<key>` can be read/written if you know the key, but the `saves` root can't be listed (verified 2026-07-07: `saves/<key>` GET → 200, root `saves` → 401), an accepted low-stakes tradeoff; optional tightening steps + setup history are in `Documents\Claude\Projects\ella-horse-game\phase3-firebase-sync-DRAFT.md`. Firebase SDK loaded via two compat `<script>` tags before `</head>`. A **manual backup** (copy/restore a base64 code) in the same panel works with no config and no internet.

## Players — "Wer spielt gerade?" (2026-07-07)
Kid-facing profile picker layered over the magic-word identity above. Goal: set a computer as Ella's, sign in elsewhere by name, and let guests keep their own save — no password, no "magic word" wording.
- **UI:** a chip under the main-page header (`#playerChip`) opens `#playerOverlay` ("Wer spielt gerade?") with a tile per known player + "➕ Neuer Spieler". Handlers (`openPlayerPicker`/`closePlayerPicker`/`pickPlayer`/`confirmNewPlayer`/`showNewPlayer`/`renderPlayerChip`) live inside the sync IIFE at the end of the main `<script>`.
- **Identity (plain-name):** the typed name is slugified (`slugName`) into the Firebase key = `ellaMagicWord`, so the whole sync module keys off the active player unchanged. Same name on any computer → same save. (Trade-off: a guest typing "Ella" would land on her save; switch to name + hidden-code if that ever matters.)
- **Registry:** `ellaPlayers` = `[{key,name}]` known on THIS device (drives the tiles). Device-local — in the sync `SKIP` set, never mirrored to the cloud.
- **Switch (`switchToPlayer`):** flushes the current push, then loads the target's cloud copy onto this computer **unconditionally** (a deliberate pick, so no timestamp guessing); `ellaMagicWord` changes only on a successful cloud read (offline switch fails safe, nothing cleared). A brand-new player = empty cloud → `clearProgress()` wipes every `ella*` progress key → fresh slate, then the empty node is claimed. Everyday reopen still uses the timestamp-guarded `pullOnce` (newest wins).
- **Migration (`migratePlayers`, once on load):** if `ellaPlayers` is missing → seed it from an existing magic word, else adopt the current local progress as player **"ella"/"Ella"** (sets `ellaMagicWord='ella'`, bumps `ellaSyncTs` so local wins and is pushed up — existing progress is never cleared or overwritten).
- **Gotcha:** switching **resets local progress** to the picked player — never test-switch on Ella's real device unless her save is already safely in the cloud. The old Grown-ups magic-word/backup UI still works as an advanced fallback but does not add to the `ellaPlayers` registry.

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
