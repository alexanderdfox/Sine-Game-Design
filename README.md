# Sine Game Design

Vector art + pure-sine sound pipeline for 2D/3D game experiments.

**Live hub:** [alexanderdfox.github.io/Sine-Game-Design/](https://alexanderdfox.github.io/Sine-Game-Design/)

## How everything works together

```
Studio  ──ZIP (XML manifest + .ogg)──►  Vector Player  (inspect / scrub / scope)
   │                                        │
   └──ZIP─────────────────────────────────► Matrix     (tile sequencer / puzzle)
                                              │
Game Studio  ◄── XML levels + sine music ─────┘
     │
     └── exports playable sine packages (game.xml + generated WAV)
```

| Module | Role |
|--------|------|
| **Studio** | Paste OpenSCAD-style points, pre-rotate XYZ, live 3D wireframe + 2D phosphor scope, export single-frame or combinatorial ZIPs |
| **Vector** | Load any Studio ZIP, list files, scrub frames, play geometry + audio together |
| **Matrix** | 4×4 sliding puzzle; drop the same ZIPs onto tiles to sequence art + sound |
| **Game Studio** | Full pure-sine engine (player, platforms, collectibles, particles, music). Edit XML, play, export ZIP packages |

Shared language: **ZIP packages containing an XML manifest + audio**. Studio produces them; Vector and Matrix consume them. Game Studio authors its own XML levels and can export complete packages.

## Files

- `index.html` — hub + pipeline overview
- `studio.html` — geometry + bulk ZIP exporter
- `vector.html` — asset player / inspector
- `matrix.html` — puzzle sequencer
- `gamestudio.html` — pure-sine game engine
- `demogame.xml` — sample platformer level (loaded by default in Game Studio)

## Quick start

1. Open `index.html`
2. In **Studio**, keep the default polyhedron (or paste your own points) → **Load Geometry** → **Export Current Frame ZIP**
3. Open **Vector** or **Matrix** and drop that ZIP in
4. Open **Game Studio** to play / edit the sine platformer and export a full package

## Demo video

https://youtu.be/Qr3VsZYQy4s
