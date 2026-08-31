# RWD Corniche — Google AI Studio one-shot prompt

Read this instead of `PROMPTS.md` for the AI Studio run. See "Corrections to
PROMPTS.md" at the bottom for what changed and why.

---

## Setup (2 minutes)

1. Go to **aistudio.google.com** → *Chat* (left sidebar) → **Create API key**
   isn't needed; the chat UI is enough.
2. Model dropdown (top right): pick **Gemini 3 Pro Image** (a.k.a. "Nano Banana
   Pro"). If you only see **Gemini 2.5 Flash Image**, that works too but holds
   detail slightly less well.
3. Open **Run settings** → set **Aspect ratio: 4:5** and **Resolution: 2K**
   (or 4K on Pro). 4:5 is the native shape of the source render — forcing 16:9
   will re-frame the building and break the sequence.
4. Upload **`SOURCE_upload_me.jpg`** (I made this — 1593×2000, 745 KB, a clean
   downscale of `RWD Corniche.jpg`). Don't upload the 23 MB original; it's slow
   and gains nothing at 2K output.
5. Turn **Temperature down to ~0.4**. Default 1.0 invites the model to
   redesign your building.

---

## The one-shot prompt

Paste this **once**, with the image attached. It generates Stage 1. Then for
every following frame you send **one short line** (listed under it) in the same
chat — the model keeps the locked description in context, so you never re-paste
the wall of text.

> This is the "one shot" in the practical sense: one prompt written once, eight
> frames out. A literal single generation containing all 8 panels in a grid is
> possible but each panel lands around 700 px wide — unusable for a full-bleed
> web hero. Don't do it.

```
You are producing an 8-frame architectural construction timelapse of a real
building, by editing the attached photograph backwards in time. I will ask for
the frames one at a time. Read these rules once and apply them to every frame
without me repeating them.

=== LOCKED — IDENTICAL IN ALL 8 FRAMES ===

CAMERA: Do not move the camera. Identical position, identical low three-quarter
street-level viewpoint looking up at the building's corner, identical lens,
identical vertical 4:5 framing, identical horizon line, identical perspective
convergence. The building must occupy exactly the same pixels of the frame in
every image. This matters more than anything else in this brief.

SURROUNDINGS: Keep unchanged in every frame — the wide asphalt road with the
yellow centre line across the foreground, the white boundary wall, the tall
street lamps, the mature shade trees at left and right, the distant low-rise
city buildings on the horizon including the beige mid-rise slab at right, and
the overall city context of Egmore, Chennai, India.

THE BUILDING (this is the finished state, for your reference — earlier frames
show it partly built, but the geometry you build toward is always this):
A roughly 15-storey residential tower composed of two tall vertical wings meeting
at a corner. The forward wing is taller and steps ahead of the rear-left wing.
Each wing is crowned by a large cantilevered flat roof canopy with a warm
timber-slatted soffit lit by recessed linear LED strips, overhanging the facade
and rising to a shallow peak at its leading edge. On the front corner runs a
full-height faceted glazed stair-and-lift shaft of stacked angled glass panels.
Immediately beside it, a full-height ribbon of ornate golden perforated jali
screen, backlit warm amber, runs unbroken from podium to crown. The main facade
is dark charcoal-grey with white-framed square punched windows, recessed
balconies and slim vertical grey fins. The lowest three levels are clad in warm
horizontal timber louvres with irregular window openings, above a dark metal
sliding entrance gate.

=== RULES ===

- Photoreal architectural photography. No illustration, no CGI look, no HDR
  halos, no oversaturation, no text, no watermark, no logo.
- Never change the floor heights, the bay spacing, the window grid or the
  proportions between frames. A floor that exists in one frame must sit at the
  same height in the next.
- No people close to camera. No extra towers or skyscrapers added to the skyline.
- Output a single photograph, no borders, no captions, no split panels.

=== FRAME 1 of 8 — EMPTY PLOT, DAWN ===

Remove the tower completely. In its place show the cleared plot: bare levelled
red-brown earth, blue site hoarding around the perimeter, a small site-office
cabin, a stack of steel and a single yellow tower crane standing alone. Time of
day 05:30 dawn — low warm orange sun near the horizon, soft mist, pale sky, no
artificial lighting anywhere. Everything else exactly as locked above.
```

### Then, one line each, same chat, in order

```
Frame 2 of 8 — FOUNDATION. Same camera, same everything. Deep excavated pit,
poured concrete raft, a dense grid of steel reinforcement bars projecting
upward, heaps of excavated soil, the tower crane beside it. Time 07:00, clear
soft morning daylight, long shadows.
```
```
Frame 3 of 8 — FRAME RISING, 4 FLOORS. Only 4 completed bare grey concrete
floors — columns and flat slabs, no walls, no cladding, no glass, no timber, no
jali. Plywood formwork and props on the top deck, scaffolding and green safety
netting around the perimeter, crane alongside. Time 09:00, bright blue sky. All
facade lighting off.
```
```
Frame 4 of 8 — CLIMBING, 9 FLOORS. 9 completed floors. Red brick and blockwork
infill walls on the lower floors, bare grey frame on the upper floors, green
safety netting wrapping the top three levels, scaffolding, crane climbed
higher. No cladding, no glazing, no timber, no jali, no roof canopies. Time
11:00, hard bright daylight.
```
```
Frame 5 of 8 — TOPPED OUT. Full height, all ~15 floors poured, blockwork
complete throughout, still raw grey and red brick. No stone, no timber louvres,
no golden jali, no glazed lift shaft, no roof canopies, no lighting. Scaffolding
fully up, crane at maximum height. Time 13:00, harsh overhead sun, short hard
shadows.
```
```
Frame 6 of 8 — FACADE GOING ON. Dark charcoal facade panels, window frames and
glazing installed on the lower two thirds; the golden jali ribbon and the
timber podium louvres installed on the lower third only; raw grey blockwork
still exposed on the top third. Glazed lift shaft half complete. Scaffolding
partly struck, crane still there. Roof canopies not yet built. All facade
lighting off. Time 16:00, warm raking side light.
```
```
Frame 7 of 8 — COMPLETE, GOLDEN HOUR. Structurally finished exactly as the
locked description: full-height golden jali ribbon, full glazed lift shaft,
both timber-soffit roof canopies in place, timber podium, all glazing in.
Scaffolding and crane gone, palms and landscaping planted, boundary wall and
gate finished. Time 18:30 golden hour — warm amber sun, long shadows, glowing
sky. The amber facade LEDs are only just beginning to glow, very faint.
```
```
Frame 8 of 8 — DUSK, FULLY LIT. Identical to the attached original photograph:
deep blue-to-peach dusk sky, every amber LED strip and jali panel glowing,
apartment windows lit, canopy soffits lit, street lamps on, car light trails on
the road. Match the original exactly.
```

**Frame 8: don't generate it.** Use `RWD Corniche.jpg` itself. It's your best
asset and it's real. The prompt above is only there if a regeneration is needed
for consistency reasons.

---

## Negative prompt

AI Studio's chat has no negative field. If you use the API or a tool that does:

```
morphing, melting geometry, warped windows, changed floor count, changed camera
angle, glass skyscraper, western downtown skyline, added high-rise towers,
cartoon, illustration, 3D render, CGI, oversaturated, HDR halos, watermark,
text, logo, people close to camera, fisheye, distorted architecture
```

---

## Test before you burn a session

Generate **1, 5 and 7 first**. Put them side by side. If the building isn't
recognisably the same building across those three, stop and fix the prompt —
don't generate the other five.

When a frame drifts, the fix is almost always: *"Regenerate frame N. The camera
moved / the building got wider. Match the attached original's framing exactly."*
and re-attach `SOURCE_upload_me.jpg`.

---

## Checklist

- [ ] All 8 at 4:5, same pixel dimensions
- [ ] Camera identical — flip between them fast; the horizon must not jump
- [ ] Road, wall, street lamps, trees, background skyline consistent
- [ ] Floors: 0 → 0 → 4 → 9 → 15 → 15 → 15 → 15
- [ ] Facade lighting: off in 1–6, faint in 7, full in 8
- [ ] Roof canopies absent until frame 7
- [ ] Golden jali: absent 1–5, lower third in 6, full height 7–8
- [ ] Crane present 1–6, gone in 7–8
- [ ] Export each as JPEG, ~1600 px wide, under 400 KB → `public/frames/stage-01.jpg` … `stage-08.jpg`

---

## Corrections to PROMPTS.md

Three things in that file will cost you a session if you follow it as written:

1. **Wrong source image.** It says to upload `images.jpeg` — that file is
   **568×538**, a listing thumbnail. Every output would inherit its mush. Use
   `SOURCE_upload_me.jpg` (from `RWD Corniche.jpg`, 5826×7314).

2. **Wrong building.** It describes "cream stone cladding", "two vertical
   blades" and "open hollow rectangular pergola crown frames". The actual render
   is a dark charcoal facade with a golden backlit jali ribbon, a faceted glazed
   lift shaft, timber podium louvres, and solid cantilevered canopies with
   timber-slatted lit soffits — not pergolas. Prompting for stone would have
   produced a different building at frame 7 than the one at frame 8.

3. **Wrong aspect ratio.** The checklist asks for 16:9. The source is 4:5
   portrait. Generate at 4:5 and crop in CSS if the layout needs a letterbox —
   don't make the model re-frame.

Also: it calls the original a "photograph". It's a **CGI marketing render**.
Doesn't change the method, but tell the model *photoreal architectural
photography* (as the prompt above does) or you'll get render-of-a-render.

**Licensing still stands** — confirm with RWD that you have rights to use their
render as a generation source, and check Google's current commercial-use terms
for the tier you're on. Outputs carry SynthID watermarking.
