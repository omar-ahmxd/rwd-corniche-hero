# Anchor prompts

Clip prompts describe **motion only**. Anchor stills are the opposite job — here you *do*
describe the building, because these images are what define it for every clip downstream.

**Tool:** Nano Banana Pro. Image generation is free on Google AI Pro. Generate 4-6 of
everything and pick; it costs nothing.

**Watermarks:** Flow and the Gemini app stamp a visible sparkle. Generate anchors on a surface
that does not (AI Studio, or Kling Image Generation on the paid plan) — `sequence-clean/` was
produced this way. Never strip a mark; regenerate instead.

**Verify every subtraction with the diff** in `CLAUDE.md`. The change must be confined to the
plot. Eyeballing it is how footprints drift.

---

## 1. Master day plate — the source everything else derives from

Generate this once, at 16:9. Everything else is subtraction backwards or relighting forwards.
Attach `gen2.jpeg` and `anchors/D-asbuilt-front.jpg` as references.

> High-end architectural visualisation of the residential tower in the reference images.
> Elevated three-quarter aerial view from about a hundred and fifty metres, looking slightly
> down at the building from across a street intersection. Wide-angle, the tower standing tall as
> the clear hero of the frame with the city spreading to the horizon behind it.
>
> THE BUILDING — match the references exactly. Two tall slender wings splaying outward in a
> shallow V and meeting at a recessed central slot, about fifteen storeys. Facade of dark
> charcoal panels, with warm timber-toned vertical strips running the full height at the inner
> edge of each wing, and grey geometric perforated jali screen panels on the outer faces.
> Irregular punched square windows. Open stilted ground floor on white columns with a landscaped
> forecourt and a dark metal entrance gate. Each wing is topped by its own OPEN HOLLOW
> RECTANGULAR PERGOLA CROWN FRAME — two separate crowns, sky clearly visible straight through the
> openings, not solid roofs.
>
> THE SETTING — an idealised, immaculate version of a dense Chennai neighbourhood. Tree-lined
> avenue running past the tower with tidy traffic, mature rain trees and coconut palms, clean
> wide pavements with people walking, low-rise cream and white apartment blocks in the middle
> distance, a landscaped park with lawns to one side, a soft hazy city skyline on the horizon.
> No stadium, no lake, no landmark buildings — an ordinary Indian city made beautiful, not a
> foreign masterplan.
>
> Bright clear day, deep blue sky with thin wispy cirrus cloud, warm sunlight raking across the
> facade, crisp shadows. Immaculate professional real estate marketing render, V-Ray quality,
> ultra sharp, fine material detail, clean edges, perfect lighting, richly saturated greens,
> photoreal but idealised.

Negative:

> solid roof, filled crown, closed canopy, single block tower, brick facade, cream facade,
> stadium, lake, footbridge, curved white pavilion, eight-lane boulevard, western skyline, Dubai,
> Shanghai, slums, clutter, rubbish, wires, blurry, low detail, warped architecture, melting
> geometry, cartoon, text, watermark, logo

Judge on two things only: **both crowns open**, and **two wings, not one block.** Everything
else is fixable by editing; those two are the identity.

---

## 2. Subtraction — complete -> topped out -> mid-rise -> foundation -> empty

Each step edits the **previous** image and removes something. Attach the plate being edited,
plus the foundation plate as a second reference so the crane stays put.

Template — adjust the storey count per stage:

> Edit this image. Keep the camera position, the framing, the city, the streets, the traffic,
> the trees, the parks, the surrounding buildings, the skyline, the sky and the daylight EXACTLY
> as they are. Change only the tower on the central plot.
>
> Replace the finished tower with the same building under construction, topped out at roughly
> TEN storeys — about two thirds of its final height. It must stand on exactly the same
> footprint, with exactly the same width, the same depth and the same bay spacing as the
> finished tower it replaces. Do not make it wider, narrower or shift it on the plot.
>
> At that stage it is bare grey concrete structure only: square columns and flat floor slabs
> with open floors between them, and nothing else. No facade, no cladding panels, no glass, no
> windows, no timber, no balconies, no roof canopy. Plywood formwork and yellow props stand on
> the open top deck. Metal scaffolding and green safety netting wrap the lower two thirds.
> Construction workers on the decks.
>
> A red and yellow tower crane stands at the LEFT edge of the plot with its jib pointing right
> across the building, in the same position and the same colours as the crane in the second
> reference image. Blue site hoarding runs around the plot boundary at street level, with
> material stockpiles, site cabins and a parked concrete pump inside it.

Negative:

> finished building, facade, cladding, glass, windows, balconies, roof canopy, different
> building, wider tower, narrower tower, tower moved, changed camera, changed city, changed sky,
> night, dusk, empty plot, cartoon, text, watermark

**The two lines that carry the weight:** *"exactly the same footprint... same width, same depth,
same bay spacing"* (this is the drift), and the facade exclusions stated as an explicit list
(models read "under construction" as permission to leave some cladding on).

**Keep the crane on the same side in every stage.** A crane that jumps left to right between
plates reads badly even at time-lapse speed.

---

## 3. Relighting — day -> golden hour -> dusk -> night

Same pattern, editing the complete plate. Change the light, hold everything else:

> Edit this image. Keep the building, the camera, the framing, the city, the streets, the trees
> and every element EXACTLY as they are. Change only the time of day and the lighting.
>
> [ dusk: ] Deep indigo sky above grading to muted dusty orange at the horizon. Low contrast,
> soft moody atmosphere, no harsh light. Apartment windows beginning to glow warm, streetlights
> and landscape lighting coming on, traffic starting to trail.
>
> [ night: ] Dark blue night sky with stars. Apartment windows lit warm throughout, facade
> lighting on, streetlights and landscape lighting on, traffic as long red and white light
> trails along the roads.

---

## 4. What the real building looks like

If you are rebuilding for client delivery rather than for the pipeline test, this is the brief.
Source: `anchors/D-asbuilt-front.jpg`, a real construction photograph.

- Two tall wings splaying outward in a shallow V, meeting at a recessed central slot
- **Dark charcoal** panels — not cream. `images.jpeg` reads cream only because the perforated
  screens are backlit amber at night
- Warm timber-toned vertical strips at the inner edge of each wing, full height
- Grey geometric perforated jali on the outer faces
- Irregular punched square windows
- Open stilted ground floor on white columns
- **Two open hollow rectangular pergola crowns**, one per wing, sky visible through them

`gen2.jpeg` is the best existing render of this — dusk, front elevation, real Egmore, 2752x1536.
`gen3.jpeg` is the matching bare plot. Both were generated in Flow and carry the sparkle;
regenerate on a clean surface before use.
