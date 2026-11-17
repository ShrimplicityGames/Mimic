# MIMIC — Game Off 2026 prototype

**Theme: SHIFT.** A Spore-like shapeshift fish game. You start as a tiny mimic
fry: eat what's smaller than you, dodge what's bigger, and cheat the size
ladder by shifting your appearance.

## How to play

- **Drag anywhere to swim** (mouse works too; WASD/arrows also steer).
- **Eat** fish smaller than you. Each eat fills the growth bar.
- **Dodge** bigger predators — when one spots you the screen edges flash red
  and a `!` appears over it. A bite costs a heart (3 hearts per run).
- **BLEND** (left button / Space): shapeshift into the background — Pebble,
  Coral, Seaweed, or Rock depending on stage. Predators lose track of you,
  prey stop fleeing, but you swim slower and the meter drains. It refills
  when you un-shift.
- **LURE** (right button / F): flash a lure that pulls nearby prey toward
  you for 3 seconds (5s cooldown). The lure upgrades per stage: Wiggle →
  Morsel → Angler Light → Giant Shadow (apex lure makes predators flee).

## Growth — 4 stages

| Stage | Size | Eats to grow | Blend form | Lure |
|---|---|---|---|---|
| 1 Fry | 12 | 10 | Pebble | Wiggle |
| 2 Reef Fish | 18 | 16 | Coral | Morsel |
| 3 Hunter | 26 | 22 | Seaweed | Angler Light |
| 4 Apex Mimic | 36 | — | Rock | Giant Shadow |

Growing heals +1 heart. At Apex, predators smaller than you flee — and
become edible. Progress saves to localStorage: best score, top stage reached.

## Play locally

```sh
cd mimic-prototype/scratch/mimic
python3 -m http.server 8901
# open http://localhost:8901/ (phone-size viewport, 390x844 works well)
```

Single file, no build step, no dependencies. Canvas is 270x480 internally,
scaled up with `image-rendering: pixelated` for the chunky 8-bit look.

## Test hooks

`window.MIMIC` exposes state for headless play-testing:
`MIMIC.state`, `.score`, `.hearts`, `.chased`, `.player`, `.fish`,
`.start()`, and `MIMIC.debug` (`autoplay`, `setTarget`, `setTimeScale`,
`teleport`, `setStage`, `blend`, `lure`, `spawnNear`).

## Why Shiftfin was scrapped (do not repeat)

Shiftfin was a slow idle wait — nothing to do in the first minute. Mimic
fixes that: prey are dense from second one (3 edible fish spawn next to the
player), the first growth stage takes ~8 eats, the lure is usable
immediately, and a predator is hunting from the start. Fun is verified by
actually playing the first minute headless (see test script), not by guessing.
