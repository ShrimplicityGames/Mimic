# MIMIC — Game Off 2026

**Theme: SHIFT.** A Spore-like shapeshift fish game. You start as a tiny mimic
fry: eat what's smaller than you, dodge what's bigger, and cheat the size
ladder by shifting your appearance.

## How to play

- **Analog stick** (bottom left) to swim — the reef scrolls sideways AND down.
- **Eat** fish smaller than you. Each eat fills the growth bar. Bigger fish
  (at least 60% of your size) count double.
- **Dodge** bigger predators — when one spots you the screen edges flash red
  and a `!` appears over it. A bite costs a heart (3 hearts per run).
- **BLEND** (left button / Space): shapeshift into the background — Pebble,
  Coral, Seaweed, or Rock depending on stage. Predators lose track of you,
  prey stop fleeing. While blended you are invincible BUT you can't eat.
  Blending forces you to the seafloor.
- **LURE** (right button / F): flash a lure that pulls nearby prey toward
  you for 3 seconds (5s cooldown). The lure upgrades per stage: Wiggle →
  Morsel → Angler Light → Giant Shadow. The lure is stronger in the dark —
  a flashing light in pitch black pulls everything, prey and predators alike.
- **DASH** (button above LURE / Shift): a burst of speed to catch fast prey
  or escape a predator (2.5s cooldown).
- **DIVE**: swim down through 3 depth zones — THE SHALLOWS, THE MIDWATER,
  THE DEEP. It gets darker as you go lower. Deeper water holds more school
  fish (more food) and more predators (more danger). Each zone is patrolled
  by a boss fish — KING SNAPPER, THE GREAT EEL, ABYSS SHARK. You don't have
  to kill a boss; get past it or outsmart it to feed down there. Depth is
  not a level lock: the size rule gates the deep naturally (bigger fish live
  down there, and you can't survive them until you've grown).

## Growth — 12 stages, 8-bit to hyper-real

| Stage | Size | Eats to grow | Blend form | Lure |
|---|---|---|---|---|
| 1 Fry | 10 | 6 | Pebble | Wiggle |
| 2 Hatchling | 13 | 8 | Pebble | Wiggle |
| 3 Minnow | 16 | 10 | Coral | Morsel |
| 4 Sprat | 20 | 12 | Coral | Morsel |
| 5 Reef Fish | 24 | 14 | Seaweed | Glow Worm |
| 6 Glider | 29 | 17 | Kelp | Angler Light |
| 7 Hunter | 34 | 20 | Anemone | Lantern |
| 8 Stalker | 40 | 23 | Shell | Siren Glow |
| 9 Bruiser | 46 | 26 | Driftwood | Hypno Pulse |
| 10 Terror | 53 | 30 | Boulder | Phantom |
| 11 Leviathan | 61 | 34 | Sunken Log | Mirage |
| 12 True Mimic | 70 | — | Apex Rock | Giant Shadow |

Stages 1–5 are chunky pixel fish, 6–9 are smooth vector fish, and 10–12 are
photorealistic — a real red snapper, a real pink snapper, a real golden tang.
Growing heals +1 heart. At True Mimic, predators smaller than you flee —
and become edible. Progress saves to localStorage: best score, top stage.

## Play locally

```sh
cd mimic-prototype/scratch/mimic
python3 -m http.server 8901
# open http://localhost:8901/ (phone-size viewport, 390x844 works well)
```

Single file, no build step, no dependencies. Canvas is 270x480 internally,
scaled up with `image-rendering: pixelated` for the chunky 8-bit look.
Photo sprites for stages 10–12 are embedded as base64 data URIs; the
cut-out sources live in `art/` (fish10/11/12.png).

## Test hooks

`window.MIMIC` exposes state for headless play-testing:
`MIMIC.state`, `.score`, `.hearts`, `.chased`, `.camX`, `.camY`, `.depth01`,
`.zone`, `.bosses`, `.player`, `.fish`, `.stages`, `.start()`, and
`MIMIC.debug` (`autoplay`, `setTarget`, `setJoy`, `setTimeScale`,
`teleport`, `setStage`, `blend`, `lure`, `dash`, `spawnNear`, `photoReady`,
`usingPhoto`, `spawnTargets`).

QA: `qa-v3.py` (dash, vertical scroll, depth darkness, depth spawning,
bosses, photo sprites, blend rule) and `probe-boss-blend.py`.

## Why Shiftfin was scrapped (do not repeat)

Shiftfin was a slow idle wait — nothing to do in the first minute. Mimic
fixes that: prey are dense from second one (3 edible fish spawn next to the
player), the first growth stage takes ~6 eats, the lure is usable
immediately, and a predator is hunting from the start. Fun is verified by
actually playing the first minute headless (see test scripts), not by guessing.
