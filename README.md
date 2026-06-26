# Dream Ride — CarRunner

A surreal, single-file browser auto-runner: a bored kid in the back seat turns every car
ride into a wild imaginary platforming adventure across signs, fences, mailboxes, trees,
and goofy cardboard monsters.

The game is **`index.html`** — canvas + vanilla JavaScript, no build step and no
dependencies. The hero and several roadside objects use transparent PNG sprites in
**`sprites/`**; everything else (backgrounds, rails, rings, UI, particles) is drawn with
canvas shapes, and all sound is a tiny WebAudio synth. If a sprite hasn't loaded yet the
game falls back to canvas-vector art, so it always renders.

## Play locally

Serve the folder so the sprite PNGs load (browsers block `file://` image requests for
canvas): `python3 -m http.server` then visit `http://localhost:8000`. Opening
`index.html` directly still runs — it just falls back to the vector art for the hero and
objects.

## Game flow

**Intro card → Intro movie → How-to-Play / Title → Ride 1 → Destination → New Idea → Ride 2 → … → Home (ending).**

On first load the `intro_screen` card is shown as a **press-to-start gate**. Pressing any
button (or tapping) starts the **intro movie**, which slowly cross-fades (2s each) through
`cut1 → cut2 → cut3 → cut4` (the kid climbing into the back seat and the drive beginning).
The movie is **not** skippable and only plays the first time. The art lives in
`Intro_cut_scene/`.

The movie hands off to the **How to Play** screen, which doubles as the title/menu (game
title, high score, and the controls in two columns: what you can do *from the very first
ride*, and the *one new power you unlock at each destination*). After every ride, an upgrade
screen introduces that ride's new ability **and explains exactly how to score points with
it**. Returning here after a run skips straight past the one-time movie.

A ride does **not** end on a timer or a score — it ends when you physically reach the big
**destination building** (school, store, grandma's house, downtown tower, home) that rolls
in from the right. The car eases to a stop as you arrive, the family says a line, and you
earn the next New Idea.

## Controls

| Action | Keyboard | Touch |
|---|---|---|
| Move kid left / right | ← → · A D | — |
| Jump / Double Jump | Space · W · ↑ | Tap (left side) |
| Imagination Beam | X · K | Tap right side / hold to charge |
| Slide (on ground) | S · ↓ | Swipe down |
| Fast-fall / slam (in air) | S · ↓ | Swipe down |
| Speed up the car | Shift (hold) | — |
| Pause | P · Esc | Tap when paused |
| Mute | M | — |

The family car rides fixed at the bottom of the screen with the kid in the back seat; an
**imagination beam** runs from the back seat up to the avatar. You steer the avatar **freely
left/right** while it auto-runs — jump, slam down, and grind rails (automatic — just land on
one) as the scenery scrolls past.

## Moves & how they score

**From the start**

- **Jump** over people, animals, and **gaps** in the road.
- **Slide** under low "SLIDE" posts and signs.
- **Fast-fall** — tap Down in mid-air to slam back to the ground so you can jump again sooner.
- **Sprint** (hold Shift) to blaze across **gaps** for a **+150** bonus (you can also just
  jump them). Walking into a gap on foot drops you in and costs a Focus.
- **Bounce** off mailboxes (+75) and hydrants (+90) by landing on top.
- **Grind** fences and wires for points every tick.
- **Dodge** people/animals closely for a +100 Perfect Dodge — never attack them.

**Unlocked one per destination** (each upgrade screen explains its scoring)

1. **Imagination Beam** (X) — zap bushes & cardboard monsters, +50 each.
2. **Double Jump** — reach high rings (+100) and rooftop rails.
3. **Spark Magnet** — sparks (+10) drift into you automatically.
4. **Charged Beam** (hold X) — clear a whole row in one blast.
5. **Dream Dash** — your Sprint gets faster and passes safely through hazards.

Chaining actions close together builds the **Daydream Chain** multiplier (up to 6×).

## Other systems

- 5 themed rides (School → Grocery → Grandma's → Downtown → Night Drive Home), each with
  its own palette, skyline, spawn table, parent/child dialogue, and destination building.
- Collectibles: sparks, jump-through rings, and special **memory tokens** per destination.
- A forgiving **Focus** (health) system — taking a hit costs a Focus and causes a brief
  stumble (a temporary slowdown that recovers on its own), never a game over, so the ride
  always finishes.
- Particles (confetti, stars, smoke), screen shake, the car-window foreground frame, and a
  high score saved to `localStorage`.

## Publish on GitHub Pages

This folder is self-contained, so any of these work:

**Option A — repo root.** Put `index.html` at the root of a repo, then in the repo's
**Settings → Pages**, set Source = `Deploy from a branch`, Branch = `main` / `/ (root)`.

**Option B — `/docs`.** Move `index.html` into a `docs/` folder and set Pages to serve
from `main` / `/docs`.

**Option C — this subfolder.** Keep it here and the game will be reachable at
`https://<user>.github.io/<repo>/dream_ride/`.

Pages serves static files directly — no configuration needed beyond pointing it at the
folder that contains `index.html`.
