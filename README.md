# Sine Game Design

Vector art + pure-sine sound pipeline for 2D/3D game experiments.

**Live hub:** [alexanderdfox.github.io/Sine-Game-Design/](https://alexanderdfox.github.io/Sine-Game-Design/)

## How everything works together

```
Studio  ──ZIP (XML manifest + .ogg)──►  Vector Player  (inspect / scrub / scope)
   │                                        │
   └──ZIP─────────────────────────────────► Matrix     (tile sequencer / puzzle)

Game Studio  ◄── pure XML levels (3D sine + dual controls) ──► export ZIP
```

| Module | Role |
|--------|------|
| **Studio** | OpenSCAD-style points → live 3D/2D vector preview → single-frame or combinatorial ZIP |
| **Vector** | Load Studio ZIPs, scrub frames, phosphor scope |
| **Matrix** | 4×4 puzzle; drop the same ZIPs onto tiles to sequence art + sound |
| **Game Studio** | **3D pure-sine XML engine** — dual players, custom controls, camera, entities, music |

## Game Studio (XML-only 3D)

Everything is defined in XML:

- **Players** (1 or 2) with position, sine shape, and nested `<Controls>`
- **Custom key bindings** per player (`left`, `right`, `forward`, `back`, `up`, `action`)
- **3D entities**: `platform`, `collect`, `npc`, `particle` with `x y z`
- **Camera** with orbit (drag mouse), FOV, look-at
- **Music**, messages, background layers

### Example dual-player controls

```xml
<Player id="p1" x="-60" y="30" z="0" amp="14" color="#22d3ee" ...>
  <Controls left="KeyA" right="KeyD" forward="KeyW" back="KeyS" up="Space" action="KeyE"/>
</Player>
<Player id="p2" x="60" y="30" z="0" amp="14" color="#f472b6" ...>
  <Controls left="ArrowLeft" right="ArrowRight" forward="ArrowUp" back="ArrowDown" up="Enter" action="ShiftRight"/>
</Player>
```

Use any KeyboardEvent `code` values. Movement is camera-relative.

## Files

- `index.html` — hub
- `studio.html` — geometry + ZIP exporter
- `vector.html` — asset player
- `matrix.html` — puzzle sequencer
- `gamestudio.html` — 3D sine XML game engine
- `demogame.xml` — dual-player 3D arena demo

## Quick start

1. Open `index.html` → **Game Studio**
2. Press **▶ Play** — P1 WASD+Space, P2 Arrows+Enter
3. Drag mouse to orbit camera
4. Edit XML → **Load XML** to change levels, controls, entities
5. **Export ZIP** for a complete package

## Demo video

https://youtu.be/Qr3VsZYQy4s
