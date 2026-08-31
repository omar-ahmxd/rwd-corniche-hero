# Clip prompts

**Prompt the motion only.** The two anchor images already define the building completely.
Describing it again gives the model permission to reinterpret it — which is the exact failure
this whole approach exists to prevent. No "modern tower", no "timber louvers", no
"photorealistic architectural visualization". Motion and light, nothing else.

**Tool:** Google Flow, **Veo 3.1 Quality**, **Frames** mode (start + end), **16:9**, **1080p**,
**8s**, **audio OFF**, x1. ~100 credits per clip.

Every clip: single continuous motion, no cuts, no text.

---

## C1 — 01-foundation -> 02-frame-mid

> Accelerated construction time-lapse. Camera rises slowly and steadily. Bare concrete floors
> stack upward one after another. Tower cranes slew and lift loads. Structure only — no facade,
> no glass, no cladding. Consistent daylight throughout, clouds drift across the sky. Smooth
> continuous motion, no cuts, no text.

## C2 — 02-frame-mid -> 03-frame-topped

> Accelerated construction time-lapse. Camera continues rising slowly. Remaining concrete
> floors stack to full height. Structure only — bare grey concrete, no facade, no glass, no
> cladding. Consistent daylight throughout. Smooth continuous motion, no cuts, no text.

## C3 — 03-frame-topped -> 04-complete-day

> Accelerated time-lapse. Camera pulls back slowly. The facade forms across the structure from
> bottom to top — glass first, then cladding panels, then the roof canopy last. Scaffolding
> comes down and the cranes are removed. Consistent daylight. Smooth continuous motion, no
> cuts, no text.

## C4 — 04-complete-day -> 05-complete-night

> Time-lapse. Locked-off camera, no movement. Sunlight shifts through afternoon and golden hour
> into night. Shadows lengthen and sweep across the facade, clouds streak overhead, the sky
> deepens to dark blue. Apartment windows illuminate progressively floor by floor, streetlights
> and landscape lighting come on, traffic becomes light trails. Smooth continuous motion, no
> cuts, no text.

**Run this one first.** Locked-off camera, both anchors are true renders, lowest risk in the
set. It tests the method, not the art.

## C5 — 05-complete-night -> 06-aerial-night

> Aerial drone shot at night. Camera rises smoothly and tilts down toward the rooftop. Single
> unbroken camera motion, no cuts, no text.

---

## Negative prompt (all clips)

```
morphing, melting geometry, warped windows, rubbery structure, building sliding sideways,
changing building proportions, different building, camera shake, whip pan, cuts, cartoon,
illustration, video game, text, watermark, logo, timestamp
```

---

## Two prompt phrases that carry weight

**"Structure only — no facade, no glass, no cladding."** In C1 and C2 the end anchor is bare
frame. Without this the model knows "finished building" is where construction goes, starts
cladding early, then has to strip it back to hit the anchor. That is what makes frames flicker.

**"Consistent daylight throughout."** Both anchors in C1, C2 and C3 are daytime. Left alone,
video models drift toward golden hour because that is what archviz footage looks like in
training data.

## After each clip

Extract straight to PNG. Never re-encode.

```bash
ffmpeg -i C1.mp4 -vsync 0 -q:v 1 frames/c1_%04d.png
```

8s at 24fps is ~192 frames per clip, ~960 across the five. Delivery target is 200-300 total,
so sample roughly every fourth frame on the way to WebP. Generate dense, discard on the way out.

## What to check

One question per clip: **does the last frame match the end anchor?** Tower same width, same
height, same roof profile, windows in the same positions, building not rotated or shifted on
the plot. A beautiful clip that ends on a different building is a failure. An ugly one that
lands exactly on the anchor is a pass.

## What was tried and failed

**01 -> 07 in a single clip** (old 8-plate numbering). It dissolves — the tower fades up out of
the ground instead of building. The span is too long for one generation.

**Local generation** — every local model attempt failed. The project runs on Flow/Veo now.
See CLAUDE.md.
