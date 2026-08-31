# RWD Corniche — Construction Timelapse Prompts

Generation prompts for an 8-stage construction timelapse of RWD Corniche,
Egmore, Chennai. Every prompt below is complete and copy-pasteable — nothing
needs assembling.

**Recommended tool: Google AI Studio.** Reasoning in "Why AI Studio" below.

---

## The core idea

Do **not** describe the building and hope a model draws it. It won't be your
building — it'll be a generic tower, which is useless for selling this specific
project.

Instead: **hand the model your real photograph and edit it backwards.**
`images.jpeg` is the finished building at night. Every earlier stage is
that same photo with construction progress removed. The building's identity is
preserved because you never regenerated it.

### Two rules that decide whether this works

1. **Always edit the ORIGINAL photo, never the previous output.** Chaining edits
   compounds error — by stage 3 you have a different building. Every prompt
   below starts from `images.jpeg`.

2. **Same camera in every frame.** Identical angle, position, focal length. This
   matters more than image quality. A viewpoint that shifts between stages
   destroys the illusion, and no amount of crossfading hides it.

---

## Why AI Studio over Runway / Higgsfield

| | AI Studio | Runway / Higgsfield |
|---|---|---|
| Edits your real photo | Yes — the whole point | Limited |
| Building stays consistent | Strong | Drifts badly |
| Free tier | Generous for images | Paid |
| Video | Veo, 8s clips | 5–10s clips |

Runway and Higgsfield are better at *camera movement*. AI Studio is better at
*keeping your building your building*. For this job, identity beats motion.

---

# PART 1 — The eight stills (Google AI Studio)

Open AI Studio, choose the Gemini image model, **upload `images.jpeg`**,
then paste one prompt per generation. Re-upload the original each time.

---

### Stage 1 — Empty plot, dawn

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Remove the tower completely and show the
empty cleared construction plot in its place: bare levelled earth, site
hoarding around the perimeter, a small site office cabin, and a newly erected
yellow tower crane standing alone. Change the time of day to 5:30am dawn —
warm low orange sun near the horizon, soft mist, pale sky. Keep the
surrounding neighbourhood buildings, the road, the trees and the compound
walls exactly as they are in the original photo.
```

### Stage 2 — Foundation

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Replace the tower with foundation-stage
construction: a large excavated pit, poured concrete raft slab, a grid of
steel reinforcement bars projecting upward, heaps of excavated soil, and a
tower crane beside it. Change the time of day to 7am early morning — clear
soft daylight, long shadows. Keep the surrounding neighbourhood buildings,
road and trees exactly as they are.
```

### Stage 3 — Frame rising, 4 storeys

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Show the building under construction with
only 4 completed concrete floors. Bare grey concrete structural frame,
columns and flat slabs visible, no facade, no cladding, no glass. Plywood
formwork and props on the top deck, metal scaffolding around the perimeter,
green safety netting, a tower crane alongside. Change the time of day to 9am
morning daylight with a clear blue sky. The building's amber facade lights
are switched off. Keep the surrounding neighbourhood, road and trees
identical.
```

### Stage 4 — Climbing, 9 storeys

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Show the building under construction with
9 completed concrete floors. Grey concrete frame with blockwork infill walls
on the lower floors, bare frame on the upper floors, green safety netting
wrapping the top levels, scaffolding, tower crane climbing alongside. No
cladding, no glazing, no facade lighting. Change the time of day to 11am late
morning, bright daylight, clear sky. Keep the surrounding neighbourhood, road
and trees identical.
```

### Stage 5 — Topped out

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Show the building topped out at full
height — all floors poured, blockwork walls complete throughout, but still
raw grey with NO stone cladding, NO perforated screens, NO glazing and NO
facade lighting. The two vertical blades and the crown frames are not yet
built. Scaffolding still up, tower crane at maximum height. Change the time
of day to 1pm midday, harsh overhead sun, short hard shadows. Keep the
surrounding neighbourhood, road and trees identical.
```

### Stage 6 — Facade going on

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Show the building with its cream stone
cladding and tall perforated jali screen ribbons installed on the lower
two-thirds of the height, and raw grey blockwork still exposed on the top
third. Glazing being installed on lower floors. Scaffolding partially
dismantled, crane still present. The crown pergola frames are not yet
installed. Facade lighting switched off. Change the time of day to 4pm
afternoon, warm raking sunlight from the side. Keep the surrounding
neighbourhood, road and trees identical.
```

### Stage 7 — Golden hour, complete

```
Using this exact photograph as reference, keep the identical camera angle,
camera position, lens and framing. Show the building structurally complete
and fully clad — cream stone facade, tall unbroken perforated jali ribbons
running the full height of both vertical blades, all glazing installed, and
the open hollow rectangular pergola crown frames in place on top of both
blades. Scaffolding and crane removed, landscaping and trees planted at the
base. Change the time of day to 6:30pm golden hour — warm amber sunlight,
long raking shadows, glowing sky. The amber facade LED strips are only just
beginning to switch on, very faint. Keep the surrounding neighbourhood, road
and trees identical.
```

### Stage 8 — Night

**Use `images.jpeg` unmodified.** This is your endpoint and your best
asset — it's a real photograph. Don't regenerate it.

If you need it cleaned up:

```
Enhance this photograph: increase sharpness and clarity, reduce noise, deepen
the blue of the night sky, and slightly boost the warmth and glow of the amber
vertical LED strips. Do not change the building, the camera angle, the
composition or the surroundings in any way.
```

---

## Negative prompt

If the tool exposes a negative field:

```
morphing, melting geometry, warped windows, inconsistent floor count, glass
skyscraper, western downtown skyline, surrounding high-rise towers, cartoon,
illustration, 3D render, CGI, oversaturated, HDR halos, watermark, text,
people close to camera, fisheye, distorted architecture
```

---

# PART 2 — Motion (Veo, in AI Studio)

Optional. The stills alone work fine as a scroll-scrubbed sequence.

Upload two adjacent stills as first and last frame. **Keep prompts short** —
video models garble long descriptions.

**Construction stages (2 → 6):**
```
Locked-off timelapse. Clouds streak across the sky, the tower crane slowly
slews, shadows sweep across the facade. Camera completely static.
```

**Golden hour to night (7 → 8):**
```
Sunset into night. The sky deepens from gold to blue, amber vertical light
strips fade up the full height of the building, apartment windows switch on
one by one. Camera static.
```

**Hero shot — the only one with camera movement:**
```
Slow crane up along the facade revealing the fully lit tower against a deep
blue night sky. Smooth continuous motion.
```

Settings: **8s**, motion **low**. High motion is what makes floors melt.
Veo generates audio natively — **turn it off**. Generated construction ambience
will sound wrong; you want music or real sound design.

---

# PART 3 — Fallback: Runway / Higgsfield

Only if AI Studio doesn't work out. Both are paid.

**Runway Gen-4**, image-to-video, 5s, motion 2–3, camera motion off:
```
Locked-off timelapse. Clouds streak across the sky, crane slowly slews,
workers move on the decks, shadows sweep across the facade.
```

**Higgsfield** — let the motion preset do the camera work, keep text to what
changes:

| Stage | Preset | Prompt |
|---|---|---|
| Plot → foundation | Static | `Timelapse. Excavator works, rebar mat laid, clouds race overhead.` |
| Frame stages | Static / Slow Push In | `Timelapse. Concrete floors added one by one, crane slews, green netting rises.` |
| Facade | Crane Up | `Timelapse. Scaffolding comes down, stone cladding and perforated screens installed floor by floor.` |
| Night reveal | Slow Push In | `Sunset to night. Amber vertical lights fade on down the full height, windows illuminate.` |

**Kling** is worth knowing about: free daily credits, and its start-frame +
end-frame feature is genuinely good for exactly this job. Giving a model two
fixed endpoints to interpolate between beats asking it to invent motion.

---

# PART 4 — Checklist

Before generating all eight, generate **stages 1, 5 and 8** and put them side by
side. If the building doesn't look like the same building across those three,
the prompts need fixing before you spend more credits.

- [ ] Same camera angle and framing in all 8
- [ ] Same aspect ratio in all 8 (16:9 or 16:10) — mismatches make the
      crossfade jump
- [ ] Surrounding buildings, road and trees consistent throughout
- [ ] Floor count decreases sensibly: 0 → 0 → 4 → 9 → 17 → 17 → 17 → 17
- [ ] Facade lights OFF in stages 1–6, faint in 7, full in 8
- [ ] Crown pergola frames absent until stage 7
- [ ] Crane present stages 1–6, gone by stage 7
- [ ] 1920×1080 or larger, each file under ~400KB as JPEG

---

# PART 5 — Wiring it up

Frames go in `public/frames/` named `stage-01.jpg` … `stage-08.jpg`.

The sequence player was built and then deleted — it's about 400 lines
(Lenis scroll + canvas crossfade + Ken Burns drift + the pinned copy). Ask and
it goes back in minutes.

---

## Licensing — check before delivery

This is paid client work. Google's free-tier terms differ from paid on
commercial use, and outputs carry SynthID watermarking. Worth five minutes on
the current terms before anything goes to RWD.

Also worth confirming you have the right to use `images.jpeg` as a generation
source — the marketing renders in this folder came off a 99acres listing and
are likely RWD's own material, which is fine if RWD is the client, but confirm
it.
