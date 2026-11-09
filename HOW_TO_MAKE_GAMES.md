# How to Make Games with Sine Game Studio XML

A complete guide to authoring pure-sine 3D games using only XML.  
No external images, meshes, or audio files required — everything is geometry, motion, and sound derived from sine waves.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Quick Start](#2-quick-start)
3. [Root Element: `<SineGame>`](#3-root-element-sinegame)
4. [Camera](#4-camera)
5. [Players (1 or 2)](#5-players-1-or-2)
6. [Custom Controls](#6-custom-controls)
7. [Entities](#7-entities)
8. [Messages (UI Text)](#8-messages-ui-text)
9. [Background](#9-background)
10. [Music](#10-music)
11. [Designing Animal / Character Shapes](#11-designing-animal--character-shapes)
12. [Designing a Fighting Arena](#12-designing-a-fighting-arena)
13. [Designing a Platformer](#13-designing-a-platformer)
14. [Designing a Collectathon](#14-designing-a-collectathon)
15. [Two-Player Patterns](#15-two-player-patterns)
16. [Workflow: Edit → Load → Play → Export](#16-workflow-edit--load--play--export)
17. [Full Schema Reference](#17-full-schema-reference)
18. [Complete Example: Beast Arena](#18-complete-example-beast-arena)
19. [Tips & Gotchas](#19-tips--gotchas)

---

## 1. Overview

Sine Game Studio is a **XML-driven pure-sine engine**.

| Concept | How it works |
|--------|----------------|
| Visuals | Closed sine curves (“blobs”) projected in 3D |
| Motion | Sine parameters (`amp`, `freq`, `phase`) + physics |
| Audio | Generated sine harmonics from `<Music>` |
| Input | Per-player key maps in `<Controls>` |
| Levels | One `.xml` file = one playable game |

You never need sprites, models, or sound files. Shape and personality come from numbers.

---

## 2. Quick Start

1. Open `gamestudio.html`.
2. Paste any valid `<SineGame>…</SineGame>` into the editor.
3. Click **Load XML**.
4. Click **▶ Play**.
5. Drag the mouse on the canvas to orbit the camera.

Minimal valid game:

```xml
<?xml version="1.0"?>
<SineGame name="HelloSine" width="900" height="520" perspective="1">
  <Camera x="0" y="40" z="200" lookX="0" lookY="20" lookZ="0" fov="55" rotY="0" rotX="-12"/>
  <Player id="p1" x="0" y="30" z="0" amp="14" freq="2" phase="0" color="#22d3ee" flap="11" speed="150" mass="1">
    <Controls left="KeyA" right="KeyD" forward="KeyW" back="KeyS" up="Space" action="KeyE"/>
  </Player>
  <Entity type="platform" id="floor" x="0" y="0" z="0" amp="2" freq="0.2" phase="0" color="#64748b" width="200" depth="150"/>
  <Message text="HELLO SINE" x="340" y="30" size="20" color="#e2e8f0"/>
  <Background layers="3" speed="0.25"/>
  <Music base="100" harmonics="4" tempo="1"/>
</SineGame>
```

---

## 3. Root Element: `<SineGame>`

```xml
<SineGame name="MyGame" width="900" height="520" perspective="1">
  ...
</SineGame>
```

| Attribute | Type | Default | Meaning |
|-----------|------|---------|---------|
| `name` | string | `SineGame` | Title used in exports and HUD |
| `width` | number | `900` | Canvas width (px) |
| `height` | number | `520` | Canvas height (px) |
| `perspective` | `0` or `1` | `1` | Enable 3D projection |

All child elements are optional, but a playable game usually needs at least one `<Player>` and a floor platform.

---

## 4. Camera

```xml
<Camera
  x="0" y="55" z="240"
  lookX="0" lookY="35" lookZ="0"
  fov="52"
  rotY="0" rotX="-18"
/>
```

| Attribute | Meaning |
|-----------|---------|
| `x y z` | Camera distance / offset (mainly `z` pushes the view back) |
| `lookX lookY lookZ` | Point the camera looks at (world center of action) |
| `fov` | Field of view in degrees (45–70 typical) |
| `rotY` | Yaw in degrees (orbit left/right). Mouse drag also changes this live |
| `rotX` | Pitch in degrees (look up/down). Negative = look slightly down |

**Tips**
- Put `lookY` near player height so fighters stay framed.
- Larger `z` = more zoomed out.
- Start with `rotX="-15"` to `"-20"` for a good arena angle.

---

## 5. Players (1 or 2)

You may define **one or two** `<Player>` elements. Each becomes an independent controllable sine creature.

```xml
<Player
  id="wolf"
  x="-85" y="28" z="10"
  amp="18" freq="1.6" phase="0.2"
  color="#38bdf8"
  flap="13" speed="145" mass="1.15"
>
  <Controls left="KeyA" right="KeyD" forward="KeyW" back="KeyS" up="Space" action="KeyE"/>
</Player>
```

### Player attributes

| Attribute | Role | Suggested range |
|-----------|------|-----------------|
| `id` | Unique name (`p1`, `wolf`, `fox`…) | required for clarity |
| `x y z` | Start position in 3D | arena-sized: roughly ±150 |
| `amp` | Blob size / “body mass” visual | 10–22 |
| `freq` | How fast the silhouette oscillates | 1.0 (heavy) – 3.5 (nervous) |
| `phase` | Offset so two players don’t look identical | 0 – 6 |
| `color` | Stroke/glow color (hex) | any `#rrggbb` |
| `flap` | Jump strength | 9–16 |
| `speed` | Ground move speed | 120–200 |
| `mass` | Gravity multiplier | 0.8 (light) – 1.3 (heavy) |

### How animals emerge from numbers

| Creature feel | amp | freq | mass | speed | color ideas |
|---------------|-----|------|------|-------|-------------|
| Wolf / bear | high | low | high | medium | blue, brown, steel |
| Fox / cat | medium | high | low | high | orange, amber |
| Bird / raven | low–med | high | low | high | purple, white |
| Turtle / tank | high | very low | high | low | green, slate |

The engine draws a closed sine curve; different `amp`/`freq`/`phase` make it feel stocky, lean, twitchy, or calm.

---

## 6. Custom Controls

Controls are **per player** and use browser `KeyboardEvent.code` values.

```xml
<Controls
  left="KeyA"
  right="KeyD"
  forward="KeyW"
  back="KeyS"
  up="Space"
  action="KeyE"
/>
```

| Binding | Game action |
|---------|-------------|
| `left` / `right` | Strafe (camera-relative) |
| `forward` / `back` | Move toward / away from camera look direction |
| `up` | Jump |
| `action` | “Power” — briefly inflates the sine blob (bite / howl / pounce feel) |

### Common codes

| Key | Code |
|-----|------|
| A S D W | `KeyA` `KeyS` `KeyD` `KeyW` |
| Arrow keys | `ArrowLeft` `ArrowRight` `ArrowUp` `ArrowDown` |
| Space | `Space` |
| Enter | `Enter` |
| Left/Right Shift | `ShiftLeft` `ShiftRight` |
| E Q F | `KeyE` `KeyQ` `KeyF` |
| Numpad | `Numpad0` … `Numpad9` |

**Classic dual layout**

```xml
<!-- Player 1 -->
<Controls left="KeyA" right="KeyD" forward="KeyW" back="KeyS" up="Space" action="KeyE"/>

<!-- Player 2 -->
<Controls left="ArrowLeft" right="ArrowRight" forward="ArrowUp" back="ArrowDown" up="Enter" action="ShiftRight"/>
```

You can remap anything — e.g. both players on one keyboard with different zones, or one player on WASD and another on IJKL.

---

## 7. Entities

Entities are everything that is not a player: platforms, collectibles, NPCs, particles.

```xml
<Entity
  type="platform"
  id="ring"
  x="0" y="18" z="0"
  amp="4" freq="0.5" phase="0.3"
  color="#64748b"
  width="140" depth="90"
/>
```

### Shared attributes

| Attribute | Used by | Meaning |
|-----------|---------|---------|
| `type` | all | `platform` \| `collect` \| `npc` \| `particle` |
| `id` | all | Unique name |
| `x y z` | all | World position |
| `amp` | all | Size / wobble strength |
| `freq` | all | Animation speed |
| `phase` | all | Time offset |
| `color` | all | Hex color |

### Type-specific attributes

| Type | Extra attrs | Behavior |
|------|-------------|----------|
| `platform` | `width`, `depth` | Walkable (visual) rectangle; slight sine wobble |
| `collect` | — | Touch to score (+10). Disappears when taken |
| `npc` | — | Decorative sine creature (spectators, referees) |
| `particle` | `count` | Swarm of small sine points (dust, embers, magic) |

### Entity design notes

**Platforms**
- Always include a large floor (`width` 250–350, `depth` 150–220).
- Stack height with `y` for multi-level arenas.
- Corner perches: smaller `width`/`depth`, higher `y`.

**Collectibles**
- Place above platforms (`y` a bit higher than platform top).
- Faster `freq` (2.8–3.6) makes them sparkle.
- Use team colors if you want “wolf orb” vs “fox orb”.

**NPCs**
- Great for crowds and personality (owl referee, bear in the stands).
- Larger `amp` + lower `freq` = big slow beasts.

**Particles**
- `count` 12–30 is enough.
- One cool-colored swarm + one warm swarm sells “arena energy”.

---

## 8. Messages (UI Text)

```xml
<Message text="SINE BEAST ARENA" x="300" y="24" size="22" color="#f1f5f9"/>
<Message text="WOLF vs FOX" x="340" y="48" size="12" color="#94a3b8"/>
```

| Attribute | Meaning |
|-----------|---------|
| `text` | String to show (avoid `"` inside, or escape) |
| `x y` | Screen position (pixels, not world) |
| `size` | Font size |
| `color` | Hex |

Letters bounce slightly with sine — keep titles short.

---

## 9. Background

```xml
<Background layers="4" speed="0.18"/>
```

| Attribute | Meaning |
|-----------|---------|
| `layers` | Number of parallax sine hills (2–5) |
| `speed` | Scroll / wave speed of the hills |

Lower speed = calmer mood; higher = more chaotic fight energy.

---

## 10. Music

```xml
<Music base="78" harmonics="6" tempo="1.15"/>
```

| Attribute | Meaning | Feel |
|-----------|---------|------|
| `base` | Fundamental frequency (Hz) | 60–90 dark, 100–140 bright |
| `harmonics` | Number of sine partials | 3–4 soft, 5–7 richer |
| `tempo` | LFO / movement rate | `<1` slow, `>1` urgent |

Music is synthesized at runtime from pure sines. Press **Music** in the UI to start, **Mute** to stop.

---

## 11. Designing Animal / Character Shapes

There is no mesh system. “Shape” is the closed sine curve + motion.

### Recipe

1. Pick a **body size** → `amp`
2. Pick a **nervousness** → `freq`
3. Pick a **weight** → `mass` + `speed`
4. Pick a **color identity**
5. Offset `phase` so two characters don’t pulse in sync

### Animal presets (copy-paste)

**Wolf**
```xml
amp="18" freq="1.6" phase="0.2" color="#38bdf8" flap="13" speed="145" mass="1.15"
```

**Fox**
```xml
amp="15" freq="2.8" phase="1.4" color="#fb923c" flap="15" speed="175" mass="0.9"
```

**Bear**
```xml
amp="22" freq="0.9" phase="0" color="#a16207" flap="10" speed="110" mass="1.35"
```

**Raven**
```xml
amp="12" freq="2.4" phase="1.8" color="#7c3aed" flap="16" speed="190" mass="0.75"
```

**Owl**
```xml
amp="13" freq="1.1" phase="0.5" color="#e2e8f0" flap="12" speed="130" mass="1.0"
```

Use the same numbers on `<Player>` or on `<Entity type="npc">`.

---

## 12. Designing a Fighting Arena

Checklist:

1. **Floor** — large platform at `y="0"`.
2. **Center ring** — slightly raised platform for the main clash.
3. **Corner perches** — one per player, higher `y`, near spawn.
4. **High pad** — optional sky platform for aerial plays.
5. **Rails / walls** — thin platforms at the sides for boundary feel.
6. **Orbs** — 4–8 collectibles as “hits” / score objectives.
7. **Spectators** — 2–3 NPCs in the background.
8. **Particles** — dust + embers.
9. **Camera** — look at ring center, slight downward pitch.
10. **Messages** — title + control reminder + short objective.

Spawn players on opposite sides:

```xml
<Player id="wolf" x="-85" ...> ... </Player>
<Player id="fox"  x="85"  ...> ... </Player>
```

---

## 13. Designing a Platformer

- One player (or two coop).
- Chain platforms with increasing `y` and varying `x`/`z`.
- Put collects above each platform.
- Lower gravity feel: smaller `mass`, higher `flap`.
- Camera `lookY` follows the middle of the course; pull `z` back so more of the path is visible.

Example vertical step:

```xml
<Entity type="platform" id="p1" x="-40" y="20" z="0"  width="80" depth="50" .../>
<Entity type="platform" id="p2" x="40"  y="50" z="-20" width="70" depth="45" .../>
<Entity type="platform" id="p3" x="-20" y="80" z="-40" width="60" depth="40" .../>
```

---

## 14. Designing a Collectathon

- Scatter many `type="collect"` entities in 3D space.
- Use distinct colors per region.
- Score is automatic (+10 per orb, tracked per player id).
- Goal text in a `<Message>`: “Collect all orbs”.

---

## 15. Two-Player Patterns

| Pattern | Setup |
|---------|--------|
| Versus arena | Opposite spawns, shared orbs, separate scores |
| Coop platformer | Same side or staggered, shared goal text |
| Race | Long path of platforms, first to far collect wins by score |
| Asymmetric | One heavy/slow, one light/fast (different amp/freq/mass/speed) |

Scores are keyed by player `id`:
- `p1` / `wolf` / anything starting with player1 logic → **P1 Score**
- other id → **P2 Score**

(Use `id="p1"` and `id="p2"` if you want the HUD labels to match classic naming; custom ids still work.)

---

## 16. Workflow: Edit → Load → Play → Export

1. **Write XML** in the left editor (or in any text editor, then paste).
2. **Load XML** — parses players, entities, camera, music.
3. **▶ Play** — physics + input + drawing loop.
4. **Music** — start the sine soundtrack.
5. **Drag canvas** — orbit camera to check readability.
6. **Sync → Editor** — writes current runtime state back to XML (positions after moving, etc.).
7. **Export ZIP** — packages `game.xml` + generated sine WAV for sharing.

Also available:
- **Scope** mode — pure waveform view.
- **Wire 3D** mode — same 3D view focused on structure.
- **Reset Demo** — restores the built-in dual-player sample.

---

## 17. Full Schema Reference

```text
<SineGame name width height perspective>
  <Camera x y z lookX lookY lookZ fov rotY rotX />

  <Player id x y z amp freq phase color flap speed mass>
    <Controls left right forward back up action />
  </Player>
  <!-- repeat Player for second fighter -->

  <Entity type id x y z amp freq phase color
          [width depth]      <!-- platform -->
          [count]            <!-- particle -->
  />

  <Message text x y size color />

  <Background layers speed />
  <Music base harmonics tempo />
</SineGame>
```

All numeric attributes accept integers or decimals.  
Colors are CSS hex (`#rrggbb`).  
Unknown attributes are ignored; missing ones fall back to engine defaults.

---

## 18. Complete Example: Beast Arena

See `animal_fight.xml` in this repo for a full dual-animal fighting layout:

- Wolf vs Fox with distinct sine “body language”
- Floor, ring, perches, sky pad, rails
- Team-tinted power orbs
- Owl / Bear / Raven spectators
- Dust + ember particles
- Title + control + objective messages
- Darker, punchier music (`base="78"`, `tempo="1.15"`)

Load that file in Game Studio to study a finished design.

---

## 19. Tips & Gotchas

1. **Always give a floor** — without a platform near `y="0"`, players only fall and bounce on the soft engine floor.
2. **Don’t stack players on the same spawn** — offset `x` (and optionally `z`).
3. **Readable colors** — bright strokes on dark background; avoid very dark `#111` fighters.
4. **Phase matters** — two blobs with the same phase look like clones; offset by ~1–2.
5. **Camera first** — if the level feels wrong, fix `look*` and `rotX` before moving every entity.
6. **Action key** — hold to “power up” the blob (visual + feel); useful as bite/howl/pounce telegraph.
7. **Performance** — keep particle `count` under ~40 total; dozens of platforms are fine.
8. **XML must be well-formed** — one root, matched tags, no bare `&` in text (use `&amp;` if needed).
9. **Key codes are case-sensitive** — `KeyA` not `keya`; `ArrowLeft` not `Left`.
10. **Export often** — ZIP is the portable unit for sharing levels.

---

## Next Steps

- Remix `animal_fight.xml`: change animals, controls, or arena height.
- Build a solo collectathon with 15 orbs.
- Try asymmetric fighters (Bear vs Raven).
- Use **Studio** to export vector ZIPs, then keep using Game Studio for playable 3D sine levels — same project family, shared pure-sine philosophy.

Everything is XML. Everything is sine. Make the arena yours.
