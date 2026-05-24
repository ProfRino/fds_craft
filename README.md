# FDS Craft

[![Latest release](https://img.shields.io/github/v/release/ProfRino/fds_craft?logo=github&label=latest)](https://github.com/ProfRino/fds_craft/releases/latest)
[![GitHub Downloads](https://img.shields.io/github/downloads/ProfRino/fds_craft/total?logo=github&label=downloads&color=blue)](https://github.com/ProfRino/fds_craft/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

<p align="center">
  <img src="assets/Logo%20FDS.png" alt="FDS Craft logo" width="320">
</p>

> A Minecraft-style voxel builder that exports valid input files for **Fire Dynamics Simulator** (FDS) — the world's most popular fire CFD tool. Designed to lure unsuspecting kids, students, and curious humans into fire engineering one block at a time.

<p align="center">
  <img src="assets/PromoGiF.gif" alt="FDS Craft in action" width="720">
</p>

---

## Download

**No installation. No build step. No admin rights. No internet at runtime.**

Grab the [latest release zip](https://github.com/ProfRino/fds_craft/releases/latest), unzip it anywhere on your PC (Desktop, Downloads, USB stick, school network share — wherever), and double-click `index.html`. That's the whole setup procedure. Per-version download counts and older releases live at <https://github.com/ProfRino/fds_craft/releases>.

If you'd rather track development, clone instead: `git clone https://github.com/ProfRino/fds_craft.git`.

## What is this?

You place blocks. The app writes a [`&OBST`](https://github.com/firemodels/fds) for each one. You add a burner. The app writes `&REAC` and `&SURF` with sensible defaults. You hit **Export**, get a `.fds` file, drop it into FDS, and watch a polyurethane fire eat your scene.

It is, in fact, the gentlest possible on-ramp into a software ecosystem that normally welcomes new users with a 540-page user guide and a Fortran 90 stack trace.

## Run it

1. Open `index.html` from the unzipped folder in any modern browser.
2. Click **Start**, click in the canvas to lock the pointer, build something, hit **Export**.

That's it. Three.js and the fonts are vendored locally, so once you have the folder you can use FDS Craft offline forever. If you want to host it for a classroom, drop the folder on any static file server (GitHub Pages, Netlify, your nephew's Raspberry Pi).

## Controls

| Action | Input |
|---|---|
| Move | `W` `A` `S` `D` |
| Fly up / down | `Space` / `Shift` |
| Sprint | `Ctrl` |
| Look | Mouse |
| Place block | Right-click |
| Break block | Left-click |
| Pick material | `1` – `7`, or scroll wheel |
| Carve a HOLE | Pick HOLE, right-click any non-fire block |
| Pause / release pointer | `Esc` |

## Materials

| Block | What you get in the `.fds` |
|---|---|
| **Concrete** | `&OBST SURF_ID='CONCRETE'` — inert wall, ρ = 2300, λ = 1.8 W/m·K |
| **Wood** | `&OBST SURF_ID='WOOD'` — ρ = 450, λ = 0.14 (no pyrolysis by default) |
| **Steel** | `&OBST SURF_ID='STEEL'` — non-combustible structural surface |
| **Glass** | `&OBST SURF_ID='GLASS'` — 30 % transparent in Smokeview |
| **Burner** | `&OBST SURF_ID='BURNER'` — HRRPUA = 500 kW/m², `TAU_Q = -30 s` t² ramp |
| **Hole** | `&HOLE` — overlays an existing non-fire block to carve an opening through it |

The HOLE rule mirrors how FDS works: `&HOLE` only does something when it overlaps an `&OBST`. So in this builder a HOLE is an overlay you paint onto a block, not a thing you place in empty space.

Exports also include:
- An auto-sized mesh with sensible padding so plumes have room to develop.
- `&REAC` for polyurethane combustion (SFPE handbook defaults) when there's a burner.
- `&VENT MB='OPEN'` on five faces so the smoke can leave.
- Diagnostic slices: temperature, visibility (with soot), and an HRRPUV plane through the burner.
- Greedy X-merging — a 10-block wall becomes 1 OBST line.

## Saving your work

- **Autosave** — every 30 s to `localStorage`, restored on next page load.
- **Save / Load** — manual `.fdscraft.json` files via the overlay menu, in case you want to share a scene or version-control it.

## What this is *not*

FDS Craft exports **valid FDS syntax**. That is the only promise.

Any resemblance to real fire dynamics is a happy accident. Please get a qualified fire engineer to check the `.fds` before showing it to a building consent officer (or to anyone whose insurance you care about).

No responsibility is taken for your kids becoming a fire engineer — or, even worse, a fire scientist.

## Built with

| Library | Use |
|---|---|
| [Three.js r128](https://threejs.org/) | 3D rendering — `vendor/three.min.js` (MIT) |
| [VT323](https://fonts.google.com/specimen/VT323) | Pixel body font — Peter Hull (OFL) |
| [Press Start 2P](https://fonts.google.com/specimen/Press+Start+2P) | Pixel heading font — CodeMan38 (OFL) |
| Web Audio API | All sounds synthesised live — no samples used |

Block, fire, and smoke textures are generated procedurally at runtime in pure JS canvas, so the entire visual identity weighs less than a single PNG.

## Credits

- **Project lead** — Prof Rino Lovreglio, Massey University (fire engineering, performance-based design, FDS workflows).
- **Play tester** — Luke De Schot, University of Canterbury.
- **Logo** — created by ChatGPT Image 2.
- **Iterative pair-programming** — Claude Code (Anthropic).

## License

[MIT](LICENSE) — do whatever you like, but don't blame us when the burner exports correctly *and* your living-room model floods with simulated soot.

---

*FDS and Smokeview are developed by [NIST](https://pages.nist.gov/fds-smv/), with contributions from VTT (Finland) and other groups worldwide. This project has no affiliation with NIST — it's a fan project that just happens to speak their language.*
