# Skybloom

A vertical one-finger sky-hopper for [Pixo Arcade](https://pixoarcade.com). Pip, a little seed spirit, bounces ever upward from a morning meadow to the aurora: grow rescue flowers, chain Perfect landings into a Golden Rush, ride updrafts and the Skyvine, and stomp the brambles.

Plain HTML, CSS and JavaScript in one file (`index.html`), no build step and no dependencies. Sounds and music are generated in the browser.

## Run it

```bash
python3 -m http.server 5391
```

Open <http://localhost:5391>. Any static host works.

## URL options

| Option | What it does |
|---|---|
| `?lang=ru` | Russian |
| `?daily=YYYY-MM-DD&twist=<id>` | The daily challenge: the same sky for every player that UTC day, plus a twist (`bloomful`, `petalless`, `gusty`, `thorny`, `golden`, `drifting`) |
| `?bot` | Plays itself (attract screen, recording clips). Never reads or writes saves, never reports runs |
| `#debug` | Exposes `__sky` in the console: `warp(m)`, `give('vine' \| 'shield')`, `rush()`, `draft()`, `bramble()`, `drop()` |

## Saves (localStorage)

| Key | What it holds |
|---|---|
| `skybloom.best` | Best climb in metres |
| `skybloom.daily` | `{ day, best }` for today's challenge |
| `skybloom.stats` | Lifetime totals: `runs`, `metres`, `seeds`, `perfects`, `rushes`, `vines`, `stomps`, `shields`, `blooms`, `dailies`, `bestAlt`, `bestCombo`, `zone` (0–4) |
| `skybloom.muted` | Sound off on this device (a setting, not progress) |

## Pixo

When the game runs inside Pixo's play page it posts `run-start` and `run-end` to the parent (board `score`, or `daily` with the day). The score is the height reached in metres; `detail` carries the zone, seeds and top combo. Nothing is posted when the game runs on its own, in `?bot` or with `#debug`.

The design notes are in [DESIGN.md](DESIGN.md).
