# Villiam Thermal Companion 🐕

NZXT CAM Web Integration that shows Villiam reacting to how hard your PC is
working. Four states, driven by **load only**.

| Stress | State | Color | Villiam |
|---|---|---|---|
| < 25 | sleeping | blue | Lies down, tucks his legs, breathes, floating z's |
| 25–50 | pacing | green | Trots on the spot, ears swinging |
| 50–75 | running | amber | Faster gait, tail going |
| > 75 | MELTDOWN | red | Tears around the rim of the screen |

## Why load and not temperature

Temperatures depend entirely on your cooling. A good loop stays cool at full
tilt and would never trigger; a bad one would sit in MELTDOWN at idle. Load
percentages mean the same thing on every machine, so they pick the state.
Temps are still shown on screen — they just don't drive anything. Set
`includeTemps: true` in `CONFIG` if you'd rather fold them in once you know
your own curves.

## Setup

1. NZXT CAM → Lighting → Kraken → **Web Integration** → Custom → Settings
2. URL: `https://avrmaster.github.io/nzxt-cam-plugin/`
3. Done. CAM feeds loads to the page every second.

## Preview without CAM

Open the page in any browser — it auto-enters demo mode and cycles through all
four states. Click to fast-forward, or force it with `?demo=1`.

Pin a single state to inspect it: `?state=sleep` · `?state=walk` ·
`?state=run` · `?state=panic`

## The sprite

Villiam is drawn as inline SVG with separately articulated parts — body, head,
both ears, tail, and four legs — so his legs actually cycle and his ears swing
rather than the whole image moving as one rigid stamp. Nothing to download and
it can't 404.

**Photo upgrade path:** drop transparent cutouts named `sleeping.webp`,
`walking.webp`, `running.webp`, `panic.webp` in the repo root and they replace
the vector sprite for those states automatically. If a file is missing the
vector is used, so you can do one state at a time.

They must have **real transparency** — a rectangular photo would drag its
background around the rim during MELTDOWN. Use WebP or PNG, not GIF: GIF alpha
is 1-bit and leaves jagged fringing against the black LCD background.

## Tuning

Everything lives in the `CONFIG` object at the top of the script in
`index.html`:

- `thresholds` — where each state starts (load %)
- `hysteresis` — how far past a threshold before switching, so he doesn't
  flicker at boundaries
- `motion` — per-state sprite size, gait/bob/tail speeds, and orbit radius.
  `size` and `orbitR` are percentages of the panel's smaller dimension, so
  they scale to whatever resolution CAM reports.
