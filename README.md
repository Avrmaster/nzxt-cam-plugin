# Villiam Thermal Companion 🐕

NZXT CAM Web Integration that shows Villiam reacting to how hard your PC is
working. Four states, driven by **load only**.

| Load | State | Scene |
|---|---|---|
| < 25 | sleeping | Night. Crescent moon, twinkling stars, Villiam curled on a cushion with his tail tucked and eye shut, breathing, z's drifting up |
| 25–50 | pacing | Daylight. Sun with haze, grass scrolling past, a butterfly flapping across, ears swinging as he trots |
| 50–75 | running | Chase. Speed lines streaking by, dust kicking up behind him, a tennis ball rolling along, ground racing past |
| > 75 | MELTDOWN | Fire. Flames licking up from below, embers rising, a rotating hazard ring, the whole panel shaking, and Villiam tearing around the inside of the bezel like a hamster wheel |

Each state has its own scenery layer that cross-fades in, so the four don't
just differ by colour — they're different little worlds.

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

## Diagnosing it on the Kraken

Add `?debug=1` to the URL in CAM and a small green overlay reports what's
actually happening on the device:

```
cam:YES upd:412 hook:ok demo:off
240x240 circle fps:25
state:walk cpu:31 gpu:18
```

- `cam:no` with `upd:0` means CAM is loading the page but never calling
  `onMonitoringDataUpdate` — the page is fine, the integration isn't feeding it.
- `hook:LOST` means something replaced `window.nzxt.v1` and our callback went
  with it. A guard re-asserts it every 500ms for the first 20s.
- `demo:on` alongside `cam:YES` should never happen; real data always wins.

## The MELTDOWN wheel

At panic Villiam is thrown out to the rim and held at a fixed -90deg relative
to the orbit arm, so his paws point radially outward for the whole lap — he
runs along the inside of the bezel and goes briefly upside down at the top,
hamster-wheel style. Because he inherits the arm's rotation, that single fixed
angle is the whole trick: no counter-rotation and no mirroring needed.

`orbitR: 81` is not arbitrary. It's derived so his paws land exactly on the
ring: the sprite is 26% of the panel, its aspect is 150x105, and his paws sit
at y=95 of 105, which puts them 0.147R below the sprite centre — so the centre
has to ride at 0.96R - 0.147R = 0.813R. Change `size` and you have to
recompute `orbitR`, or his feet will float or clip through the bezel.

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
