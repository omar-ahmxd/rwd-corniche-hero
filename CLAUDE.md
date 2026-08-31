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
is unreliable across browsers and seeks are not frame-accurate.

**Start frame + end frame conditioning is non-negotiable.** Every clip is defined by two
images. The model only invents motion between them, so it cannot redraw the building. A tool
without an end-frame slot is the wrong tool — no prompt wording compensates. This has now been
measured, not assumed; see Acceptance test below.

**Clips chain: end frame of N becomes start frame of N+1.** No seams to disguise.

**Prompt the motion only.** Never describe the building in a *clip* prompt. The anchors define
it. (Anchor *stills* are a different job and do get described — see `prompts/anchors.md`.)

**Build construction stages backwards by subtraction**, from the most complete plate. Each step
edits the previous image and removes something. Generating a stage fresh makes the footprint
drift. Verify every subtraction with the diff below before accepting it.

**Generate video in Kling, not locally and not in Flow.** See the next two sections.

## Tool decision — settled 2026-08-31

**Local generation is dead.** Every local model attempt failed. The 5090 plan in the old
version of this file is obsolete; do not restart it.

**Google Flow / Veo 3.1 was tested and rejected.** Google AI Pro does not include 1080p or
watermark removal — both are Ultra entitlements at ~Rs 6,500/mo. Pro output is 720p with a
visible "Veo" wordmark burned into every frame.

**Kling 3.0 on the Standard plan ($6.99 first month) is the tool.** It clears all three
blockers on the cheapest tier: 1080p/4K, brand watermark removal, commercial use. First-last
frame is what Kling is best at.

Measured on the same two anchors, same prompt:

|                        | Veo 3.1 (Flow) | **Kling 3.0** |
|------------------------|----------------|---------------|
| resolution             | 1280x720       | **1928x1072** |
| first frame vs anchor  | 14.96 / 255    | **9.09 / 255**|
| last frame vs anchor   |  9.68 / 255    | **7.02 / 255**|

Both test clips are in `clips/` if you want to compare by eye.

**Kling settings:** VIDEO 3.0, Mode **1080p** (not 4K — anchors are 2752px, 4K adds nothing and
costs several times the credits), Length 5s, 16:9, 1 output, Native Audio off. **40 credits per
clip.** Kling 3.0 exposes no negative-prompt field; the negatives are already inlined into the
prompts as positive statements ("Locked-off camera, no movement", "Structure only — no facade").

## Acceptance test — use this, do not judge by eye

A clip passes if its last frame matches the end anchor. Measure it:

```bash
ffmpeg -i clip.mp4 -vsync 0 -q:v 1 frames/x_%04d.png
python3 -c "
from PIL import Image, ImageChops, ImageStat
a=Image.open('sequence/END.png').convert('RGB')
b=Image.open('frames/x_0121.png').convert('RGB').resize(a.size, Image.LANCZOS)
print(sum(ImageStat.Stat(ImageChops.difference(a,b)).mean)/3)"
```

**Under ~15/255 is a pass.** Much of that residual is the resolution round-trip, not drift.
Anything far above means the tool is treating the anchor as a suggestion.

The same diff, run per-third across the frame, is how subtraction edits are verified: the
change must be confined to the plot (centre ~25/255) with the city held (flanks ~12/255).

A beautiful clip that ends on a different building is a failure. An ugly one that lands on the
anchor is a pass.

## Watermarks

Google applies visible marks on the Pro tier and removes them only on Ultra — the Veo wordmark
on video, the Gemini sparkle on Nano Banana Pro stills. **The sparkle was baked into the
original anchors** and propagated into every clip generated from them.

`sequence/sequence-clean/` holds an unwatermarked regeneration of all eight plates, same camera
and same city. **Generate against those, not against `sequence/`.**

Kling's own "KlingAI 3.0" mark is removed via the download menu on the Standard plan.

Do not strip watermarks from marked assets — regenerate on a surface that does not apply them.

## Payload caps sharpness, not the model

250 frames as WebP: ~25MB at 1400px, ~50MB at 1920px, ~87MB at 2560px. A 50MB hero is
unshippable, so **1080p source already exceeds what the pipeline can carry.** If the hero looks
soft, the answer is never a bigger subscription. In order:

1. **Fewer frames, bigger** — 120 frames at 1920px weighs the same as 250 at 1400px, and scroll
   scrub only needs to feel responsive, not hit 24fps.
2. **Sharp at rest, soft in motion** — put full-resolution anchor stills at the scroll positions
   where people stop; nobody inspects detail mid-scrub.
3. **Do not run it full-bleed** — 1400px in a contained frame reads crisp; stretched across a
   27" monitor it reads soft. Free, and it is a layout decision.

Settle the hero's rendered size before generating more clips; that number decides all of the
above.

## The building in the anchors is not RWD Corniche

The whole `sequence/` set is a generic tower in a generic masterplan — eight-lane boulevards, a
cricket stadium, an artificial lake, a Zaha-style pavilion. It is not Chennai and the tower is
not Corniche. **This is accepted deliberately for its archviz quality while proving the
pipeline. It cannot ship to RWD as-is.**

The real building, from `anchors/D-asbuilt-front.jpg` (a real construction photograph):

- Two tall wings **splaying outward in a shallow V**, meeting at a recessed central slot
- **Dark charcoal** facade panels — not cream; `images.jpeg` only reads cream because the
  perforated screens are backlit amber at night
- Warm **timber-toned vertical strips** at the inner edge of each wing, full height
- **Grey geometric perforated jali** panels on the outer faces
- Irregular punched square windows
- **Open stilted ground floor** on white columns
- Each wing topped by its own **open hollow rectangular pergola crown** — sky visible straight
  through. Not a solid roof. This and the two-wing massing are the identity; everything else is
  detail.

`gen2.jpeg` is the best asset in the repo for this — the real massing, real facade, real Egmore,
at 2752x1536, generated with Nano Banana Pro from the as-built photo. Rebuild the anchor chain
from it by subtraction when the client version is due. `prompts/anchors.md` has the prompts.

## Tried and rejected

- **Local generation (ComfyUI / Wan 2.2 FLF2V)** — every attempt failed. Obsolete.
- **Google Flow / Veo 3.1** — 720p and watermarked on Pro. See above.
- **Higgsfield** — camera-motion presets are its strength; end-frame conditioning is per-model
  and not the point of the product. Pro tier ($23/mo) needed before 1080p. Wrong tool.
- **Google Earth Studio** for a space-to-Egmore opening — India has essentially no 3D building
  mesh, so only top-down and steep angles work; obliques smear. Also non-commercial/editorial by
  default with mandatory attribution. Parked.
- **Meshy / Backflip AI** — object-scale props and scan-to-CAD. Neither handles buildings.
- **Single clip across the whole span** — dissolves rather than builds.
- **Cross-dissolving two stills** as the hero — too cheap-looking. Fallback rig in `web/`.
- **16:9 cropping the square renders** — destroys either the pergola crown or the base. Outpaint
  sideways, never crop vertically. `SHOT_F_ending_16x9.jpg` is an example of this mistake.
- **Gemini app / Flow reference tray for video** — references are not end-frame conditioning.

## Current state

- `sequence/` — six anchors, renumbered contiguously, still carrying the Gemini sparkle
- `sequence/sequence-clean/` — eight unwatermarked plates, **use these**
- `clips/` — C4 generated twice, Veo and Kling, for comparison
- Clips generated: 1 of 7 (C4 only, as the method test — it passed)
- Canvas rig built and working (`web/corniche-hero.html`)
- Kling Standard plan, ~711 credits left. 7 clips = 280 credits.

## Next action

Regenerate **C4** against `sequence-clean/` (the current one used the watermarked anchors), then
work through the remaining six. Run the diff on each before moving on.

## Open issues

- `sequence-clean/04-frame-topped.png` still has a **stray excavation pit** at the tower base —
  wrong for a topped-out stage, and it will read as the site being dug up after topping out. One
  subtraction edit fixes it. Do this before generating C3 and C4.
- Rooftop orbit has no end anchor; it needs 3-4 rotational plates, not one viewpoint.
- **The tower is not Corniche and the city is not Egmore.** Rebuild from `gen2.jpeg` before
  client delivery. This is the single biggest gap between the repo and something deliverable.
- Indian real-estate advertising rules are specific about depicting a project's surroundings.
  Worth one conversation with RWD before a film showing a fictional neighbourhood ships.
- Client still owes: full-size `-Hi-res` originals, photography of the delivered building,
  current unit sizes and price sheet, and an answer on 3D model files.

## Working notes

- Sources are 960px from a 2017 brochure PDF — soft on large displays
- Target ~1400px frames, WebP, 200-300 total — but see "Payload caps sharpness"
- Upscale after extraction, never before
- `frames/` is gitignored — ~750MB of extracted PNGs, regenerable from `clips/`
- The 60MB raw client video is gitignored; it is over GitHub's warning threshold
- Paste prompts as **one line** — copied line breaks eat spaces and produce tokens like
  "lighttrails", which degrade the result
- The user wants direct answers and working output, not options surveys
