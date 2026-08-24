# Villiam Thermal Companion 🐕

NZXT CAM Web Integration that shows different Villiam animations on the Kraken LCD
depending on CPU/GPU temperature and load.

| Stress | State | Color | File |
|---|---|---|---|
| < 25 | sleeping 😴 | blue | `sleeping.gif` |
| 25–50 | pacing 🐕 | green | `walking.gif` |
| 50–75 | running 🐕💨 | amber | `running.gif` |
| > 75 | MELTDOWN 🌪️ | red | `panic.gif` |

Missing GIFs fall back to emoji, so it works out of the box.

## Setup

1. NZXT CAM → Lighting → Kraken → **Web Integration** → Custom → Settings
2. URL: `https://avrmaster.github.io/nzxt-cam-plugin/`
3. Done. CAM feeds temps/loads to the page every second.

## Preview without CAM

Open the page in any browser — it auto-enters demo mode and cycles
through all four states. Click to fast-forward. Or force it with `?demo=1`.

## Tuning

All thresholds, temp range (35–85°C), load weight, and colors live in the
`CONFIG` object at the top of the script in `index.html`.
