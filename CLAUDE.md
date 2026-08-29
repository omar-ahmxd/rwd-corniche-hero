# Context

Read this before suggesting anything. It carries decisions already made, approaches already
tried and rejected, and the reasoning behind both. Re-litigating them wastes time.

## What we are building

A scroll-driven hero for **RWD Corniche** — a residential tower on Pantheon Road, Egmore,
Chennai. The user scrolls; the building constructs itself, day turns to night, the camera
lifts into a rooftop orbit. Scroll position is the timeline.

Developer: Omar Ahmed. Client: Ramky Wavoo Developers (RWD), a JV between Ramky Group and
Wavoo Realty. The building is **finished and occupied** — RERA TN/29/Building/0032/2018,
possession ~Aug 2021. The brochure assets are from 2017 (CorelDRAW, authored "diwa").

The construction time-lapse is a **narrative device**, not a documentary claim. This was
questioned once and settled: nobody reads a building assembling itself as a factual assertion.
Do not raise it again.

## Decisions already made

**Frame sequence on canvas, not a `<video>` element.** Scrubbing `video.currentTime` on scroll
is unreliable across browsers, seeks are not frame-accurate, and codecs degrade under
compression. A previous Veo-based attempt failed exactly this way.

**Start frame + end frame conditioning is non-negotiable.** Every clip is defined by two
images. The model only invents motion between them, so it cannot redraw the building. A tool
without an end-frame slot is the wrong tool — no prompt wording compensates.

**Clips chain: end frame of N becomes start frame of N+1.** This removes seams entirely. An
earlier plan for whip-pans and light-bloom wipes to disguise joins is obsolete — there are no
joins to disguise.

**Prompt the motion only.** Never describe the building. The anchors define it; describing it
again gives the model permission to reinterpret it.

**Build construction stages backwards by subtraction**, from the most complete plate:
`complete -> unclad -> 2/3 -> 1/3 -> foundation -> empty`. Each step edits the previous image
and removes something. Generating a stage fresh makes the footprint drift — this happened and
had to be fixed.

**Generate locally, not in the cloud.** Cloud tools return compressed MP4s you then extract
frames from, baking in compression before the pipeline starts. ComfyUI writes PNG sequences
directly. Also free, unlimited, watermark-free, Apache 2.0 — which settles commercial licensing
for a client deliverable.

## Tried and rejected

- **Meshy / Backflip AI** — Meshy is object-scale game props and hallucinates unseen faces;
  Backflip is scan-to-CAD for mechanical parts. Neither handles buildings.
- **01 -> 07 as a single clip** — dissolves rather than builds. Span too long.
- **Cross-dissolving two stills** as the hero — technically works (the day and dusk renders
  share a camera) but was rejected as too cheap-looking. Kept only as a fallback; the rig is
  in `web/`.
- **16:9 cropping the square renders** — destroys either the pergola crown or the base. The
  tower must be outpainted sideways, never cropped vertically.
- **Kling** — account banned. Appeal pending.
- **Gemini app for video** — single image slot only, so the end frame is ignored.

## Current state

- Anchors 01-08 complete, one consistent camera and city (`sequence/`)
- Clips generated: 0 of 8
- Canvas rig built and working on two plates (`web/corniche-hero.html`)
- Hardware: moving to an RTX 5090 (32GB). Previous machine was a 3070 Ti (8GB).

## Next action

Generate **C5** (`05-complete-day` -> `06-complete-dusk`). Locked-off camera, both anchors are
true renders, lowest risk in the set. It tests whether end-frame conditioning holds before
anything else is worth generating.

## Open issues

- `04-frame-topped.png` has a stray excavation pit at the tower base
- Crane jumps left-to-right between 03 and 04 (accepted — massing consistency matters more)
- C8 has no end anchor; a real orbit needs 3-4 rotational plates
- **Plates 01-04 are a generic tower, not Corniche.** Fine for proving the pipeline. Rebuild
  from `anchors/A-day.jpg` by subtraction before anything reaches the client.
- Client still owes: full-size `-Hi-res` originals from their media library, photography of the
  delivered building, current unit sizes/price sheet, and an answer on 3D model files.

## Working notes

- Sources are 960px from a 2017 brochure PDF — soft on large displays
- Payload, not model quality, caps delivery: target ~1400px frames, WebP, 200-300 total
- Upscale after extraction, never before
- The user wants direct answers and working output, not options surveys
