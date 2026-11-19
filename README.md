# MIMIC — Game Off 2026

**Theme: SHIFT.** A Spore-like shapeshift fish game. You start as a tiny mimic
guppy: eat what's smaller than you, dodge what's bigger, and cheat the size
ladder by shifting your appearance.

## How to play

- **Analog stick** (bottom left) to swim — the reef scrolls sideways AND down.
- **Eat** fish smaller than you. Each eat fills the growth bar. Bigger fish
  (at least 60% of your size) count double.
- **Dodge** bigger predators — when one spots you the screen edges flash red
  and a `!` appears over it. A bite costs a heart (3 hearts per run).
- **BLEND** (left button / Space): shapeshift into the background — Pebble,
  Coral, Seaweed, Kelp, Anemone, Shell, Driftwood, Boulder, Sunken Log, or
  Apex Rock depending on stage. Predators lose track of you, prey stop
  fleeing. While blended you are invincible BUT you can't eat. Blending
  forces you to the seafloor.
- **LURE** (right button / F): flash a lure that pulls nearby prey toward
  you for 3 seconds (5s cooldown). The lure upgrades per stage: Wiggle →
  Morsel → Glow Worm → Angler Light → Lantern → Siren Glow → Hypno Pulse →
  Phantom → Mirage → Giant Shadow. The lure is stronger in the dark — a
  flashing light in pitch black pulls everything, prey and predators alike.
- **DASH** (button above LURE / Shift): a burst of speed to catch fast prey
  or escape a predator (2.5s cooldown).
- **DIVE**: swim down into darker, deeper water. It gets darker as you go
  lower. Deeper water holds more school fish (more food) and more predators
  (more danger). Three apex predators patrol the depths — Great Barracuda,
  Tiger Shark, Goblin Shark. You don't have to kill a boss; get past it or
  outsmart it to feed down there. The game never tells you where you are or
  what patrols there — you discover the depths by swimming into them. Depth
  is not a level lock: the size rule gates the deep naturally (bigger fish
  live down there, and you can't survive them until you've grown).

## Growth — 20 stages, every one a real fish

| Stage | Species | Size | Eats to grow | Blend form | Lure |
|---|---|---|---|---|---|
| 1 | Guppy | 10 | 6 | Pebble | Wiggle |
| 2 | Betta | 11 | 8 | Pebble | Wiggle |
| 3 | Goldfish | 13 | 9 | Coral | Morsel |
| 4 | Bluegill | 15 | 11 | Coral | Morsel |
| 5 | Angelfish | 17 | 12 | Seaweed | Glow Worm |
| 6 | Discus | 19 | 14 | Seaweed | Glow Worm |
| 7 | Oscar | 21 | 15 | Kelp | Angler Light |
| 8 | Arowana | 24 | 17 | Kelp | Angler Light |
| 9 | Gar | 28 | 18 | Anemone | Lantern |
| 10 | Redtail Catfish | 31 | 20 | Anemone | Lantern |
| 11 | Giant Snakehead | 35 | 21 | Shell | Siren Glow |
| 12 | Peacock Bass | 40 | 23 | Shell | Siren Glow |
| 13 | Arapaima | 46 | 24 | Driftwood | Hypno Pulse |
| 14 | Mekong Giant Catfish | 52 | 26 | Driftwood | Hypno Pulse |
| 15 | Sawfish | 59 | 27 | Boulder | Phantom |
| 16 | Plesiosaur | 67 | 29 | Boulder | Phantom |
| 17 | Dunkleosteus | 76 | 30 | Sunken Log | Mirage |
| 18 | Mosasaurus | 86 | 32 | Sunken Log | Mirage |
| 19 | Megalodon | 98 | 33 | Apex Rock | Giant Shadow |
| 20 | Leviathan | 111 | — | Apex Rock | Giant Shadow |

All 20 stages are photorealistic — every fish is a real photo cut-out,
embedded as a base64 data URI. No 8-bit, no vector fish anywhere. Growing
heals +1 heart. At Leviathan (stage 20) you are the apex: EVERYTHING is
edible, including the bosses and the biggest predators — and everything
flees from you. Progress saves to localStorage: best score, top stage.

## Sound

- **Background music**: a slow generative underwater pad with sparse plucks,
  synthesized live with WebAudio (no audio files). Starts on the START tap,
  loops forever. **SND** button (top right) mutes music + SFX together.
- **SFX**: soft WebAudio tones and filtered noise (sine/triangle only, gentle
  attack ramps) — no clicks, no pops, no HTMLAudioElement main-thread blocks.

## Play locally

```sh
cd mimic-prototype/scratch/mimic
python3 -m http.server 8901
# open http://localhost:8901/ (phone-size viewport, 430x932)
```

Single file, no build step, no dependencies. Photo sprites for all 20 stages,
3 bosses, and 6 wild fish are embedded as base64 data URIs; the cut-out
sources live in `art/` (stage01–stage20.png, boss0–boss2.png, wild_*.png).

## Test hooks

`window.MIMIC` exposes state for headless play-testing:
`MIMIC.state`, `.score`, `.hearts`, `.chased`, `.camX`, `.camY`, `.depth01`,
`.zone`, `.bosses`, `.musicOn`, `.player`, `.fish`, `.stages`, `.start()`, and
`MIMIC.debug` (`autoplay`, `setTarget`, `setJoy`, `setTimeScale`,
`teleport`, `setStage`, `blend`, `lure`, `dash`, `spawnNear`, `photoReady`,
`photoInfo`, `usingPhoto`, `musicOn`, `musicGain`, `spawnTargets`).

QA: `qa-v4.py` + `qa-v4b.py` / `qa-v4c.py` / `qa-v4d.py` (20-stage photos,
music + mute, soft SFX, Leviathan eats everything incl. bosses, no zone
labels/lines, dash, vertical scroll, depth darkness, depth spawning, bosses,
blend rule, no console errors).

## Why Shiftfin was scrapped (do not repeat)

Shiftfin was a slow idle wait — nothing to do in the first minute. Mimic
fixes that: prey are dense from second one (3 edible fish spawn next to the
player), the first growth stage takes ~6 eats, the lure is usable
immediately, and a predator is hunting from the start. Fun is verified by
actually playing the first minute headless (see test scripts), not by guessing.
