# Glyph City

A walkable ASCII cyberpunk downtown, rendered live by a 2.5D raycaster — every
wall, window and rooftop light on screen is a character blitted from a glyph
atlas, not a pixel. Single self-contained `index.html`, no build step, no
libraries.

Inspired by [ludthor/ascii-city](https://github.com/ludthor/ascii-city) (see
the [demo video](https://www.youtube.com/watch?v=3YtygAx_C6A)) — this is an
independent build from scratch, own code throughout, aiming for the same
technique rather than reusing any of it.

## Run

Any static file server works (a classic `<script>`, not ES modules, so it
also runs directly from `file://`):

```bash
python3 -m http.server 8765
```

Open <http://127.0.0.1:8765/>.

## Controls

| Input | Action |
| --- | --- |
| **WASD** / arrow up-down | Move |
| **Shift** | Sprint |
| **Mouse** (click to lock) | Look |
| **&larr; / &rarr;** | Turn (works without pointer lock) |
| **M** | Toggle minimap |

## URL parameters

| Param | Effect |
| --- | --- |
| `?seed=demo1` | Deterministic city. Omit for a random seed (written into the URL). |

## How it works

- **Raycaster**: classic DDA grid raycasting (one ray per screen column,
  Wolfenstein-style), extended with variable building heights, floor casting
  for the street/sidewalk/park/plaza surfaces, and a screen-space horizon
  shift for mouse-look pitch (no true 3D tilt).
- **Rendering**: every character × palette-color combination is pre-drawn
  once into an offscreen canvas "atlas"; each frame just blits cells from it
  with `drawImage`, so there's no per-frame text layout cost.
- **City**: a seeded PRNG lays out an avenue/block grid, splits each block
  into quadrant lots separated by alleys, assigns each lot a height and neon
  hue, and scatters parks, a central plaza, lamps, antenna masts and a
  landmark tower.
- **Sprites**: lamps, masts, trees and the plaza tower are billboarded into
  the same character framebuffer using the standard inverse-camera-matrix
  sprite projection, depth-sorted and occlusion-tested against the wall
  raycast.

## Scope

This is a streamlined build, not a feature match: no traffic, pedestrians,
day/night cycle, or audio (all explicitly out of scope for the original's
v0.1 too, aside from audio). Focused on the rendering technique.

## License

MIT
