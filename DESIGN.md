# Skybloom — design notes (working title)

A vertical, one-finger sky-hopper for Pixo Arcade. You are **Pip**, a little seed
spirit bouncing ever upward through the sky, from a morning meadow to the aurora.

## What makes it different from a classic "jump-up" game

| Mechanic | How it works | Why it matters |
|---|---|---|
| **Bloom rescue** | Tap to grow a flower platform right under your feet. Limited charges (max 3). | Turns "I missed, game over" into a decision: save it now, or bank it for later. |
| **Perfect landings & combo** | Land in the centre of a platform → *Perfect*. Chains raise your jump height (up to +20%) and feed the bloom meter. | Skill expression: precise players climb faster and earn more rescues. |
| **Bloom meter** | Golden seeds and Perfects fill the meter; every 10 points = +1 bloom. | Links collecting, precision and survival into one loop. |
| **Altitude biomes** | Morning Meadow → Cloud Sea → Sunset Heights → Starfield → Aurora Reach. Sky, light, stars and aurora change continuously. | Visual progression; a reason to push "just a bit higher". |
| **Living platforms** | Leaf (sways), drifting cloud, dandelion puff (single-use, bursts into seeds), mushroom (super bounce), your own bloom flower. | Variety without extra controls. |
| **Bramble wisps** | Floating thorn creatures from 300 m. Stomp from above for a boost; touch from the side and you fall. | Risk/reward hazard. |

### Round 2 mechanics
| Mechanic | Where | How it works |
|---|---|---|
| **Updrafts** | from 150 m | Shimmering wind columns (chevrons at the base). Inside one, Pip is lifted up to 880 px/s. Steer into or out of them. |
| **Paper lanterns** | Sunset Heights (900 m+) | Glowing platforms that bob up and down about 26 px. Timing matters. |
| **Blink-stars** | Starfield (1600 m+) | Constellation platforms that are solid 2.5 s of every 3.4 s. They flicker before vanishing and show as a dotted ghost while off. |
| **Skyvine** (pickup) | from 120 m, rare | A vine grows under Pip and carries it about 265 m in 2.3 s. Steering still works, brambles get smashed, seeds are hoovered up. Ends with a flower burst and a jump. |
| **Petal Shield** (pickup) | from 280 m, rare | Petals orbit Pip and absorb one bramble hit. |
| **Golden Rush** | 8 Perfects in a row | For 6 s every landing counts as Perfect (so the combo keeps climbing), seeds give double meter, and the screen gets a gold frame that flickers in the last second. The next Rush needs 8 more Perfects. |

First-time popups introduce updrafts, lanterns and stars the first time you meet them in a session.
Debug (open with `#debug`): `__sky.warp(m)`, `give('vine'|'shield')`, `rush()`, `draft()`, `bramble()`, `drop()`.

### Round 3: graphics & feel
- **Zone lighting:** non-glowing world objects (leaves, clouds, puffs, mushrooms, brambles, vine, meadow) go on an offscreen layer that's tinted by `LIGHT` (warm at sunset, moonlit blue at night). Glowing things (lanterns, stars, seeds, pickups, Pip) stay bright, and Pip glows softly at night.
- **Pip:** 20% bigger (r=20). Moods: calm, happy ^^ after a Perfect, starry-eyed during Rush, thrilled on the vine, scared when falling fast, panicking when low on screen or falling away, X eyes when hit. Smooth tapered ribbon trail. The sprout blossoms (gold in Rush, pink on the vine).
- **Death:** slow-motion beat, the camera follows Pip drifting down and shrinking, the screen dims, then the card shows a sad Pip, the zone reached and the distance to your best ("So close!" within 50 m). Tap anywhere to retry (after a 0.7 s guard).
- **HUD:** the bloom bar is one compact pill, dimmed except when it matters (danger, gaining, using or failing to use a bloom). The hint text sits mid-screen. Popups nudge each other apart and stay on screen.
- **Audio:** generated music (92 BPM: pad, bass and arpeggio) with its own chord loop per zone. It goes muffled when you're in danger, gets sparkles during Rush, and ducks on death. Ambience: birds below 900 m, wind that grows with altitude, speed and updrafts, star twinkles at night. Jump pitch varies. Audio suspends when the tab is hidden.
- **Camera:** Pip is held at 50% of the screen height (was 42%) for more look-ahead.

### Round 4: second polish batch
- **Sky:** clouds catch the light (gold-lit tops and violet bellies at sunset, deep blue at night). The sun has slowly turning rays and a glow, plus a haze band at sunset. The page shows the morning sky while loading (no dark flash).
- **Background life per zone:** bird flocks (Meadow), distant floating islands with waterfalls (Cloud Sea), sky-lanterns drifting up (Sunset), a ringed planet and shooting stars (Starfield), and vertical aurora curtains (Aurora Reach).
- **Leaves:** 4 green palettes plus autumn tones at sunset, randomly mirrored, with a slight curl. **Meadow:** swaying grass blades and butterflies.
- **Zone change:** colour wash and a shower of themed sparkles, alongside the banner and chime.
- **Opening set-pieces (every run):** mushroom at 35 m, seed arc at 70 m, updraft at 110 m, Skyvine at 185 m, first bramble (off to the side, with a "Stomp from above" tip) at 240 m.
- **Menu:** compact 3-chip how-to, so Pip bounces in clear view. Fixed a bug where menu-Pip climbed onto platforms and vanished. The sun was moved out from behind the title.
- **Pause:** button in the HUD, Esc/P on keyboard, automatic when the tab is hidden. Tap anywhere to resume.
- **Combo:** a "×N" badge rides beside Pip. The top pill now only shows Golden Rush.
- **Seeds:** quick pickups climb a musical scale. **Screen wrap:** both edges glow when Pip is close to one.

## Controls (phone, portrait)
- **Drag anywhere** left/right to steer (relative drag — your finger never covers Pip). Screen wraps at the edges.
- **Tap** (or touch with a second finger) to bloom.
- Desktop: ← → / A D to steer, Space to bloom, Enter to start.

## MVP scope (this build)
Single `index.html`, no dependencies, canvas 2D + Web Audio synth sounds, best score in localStorage.

## Feel tuning (constants at the top of the game state in index.html)
- `APEX_V` / `APEX_G`: softer gravity near the top of each jump. This gives a little hang time to line up landings.
- `MAXFALL` 1300: capped fall speed, so there's time to react and bloom.
- `EDGE_TOL` 11: landing forgiveness past a platform's edge.
- `PERFECT_FRAC` / `PERFECT_PAD`: size of the Perfect zone, marked by the dewdrop in the centre of leaves and clouds.
- Tap-to-bloom only fires on a clean tap (<6 px movement, <220 ms), so steering nudges don't waste flowers.
- Blooming while rising: the flower rides under Pip and plants itself at the top of the jump.
- Feedback: short slow-motion on Perfect, stomp and bloom; a ring on landing; dew splash; a pink edge tint and pulsing flower slots when falling low with a bloom available.

## Ideas for the full game
- Sideways crosswind bands.
- Something unique for Aurora Reach (e.g. aurora ribbons you can ride).
- Seed shop: cosmetic Pip skins, flower types (bouncier bloom, double bloom).
- Daily climb with a fixed seed + leaderboard on Pixo Arcade.
- Boss-ish moments: storm cloud that chases you for 100 m.
- Haptics (`navigator.vibrate`) on Perfect / bloom.
- Optional tilt control.
