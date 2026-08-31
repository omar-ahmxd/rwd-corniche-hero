# RWD Corniche — "Earth to Egmore" construction film

Space → India → Tamil Nadu → Chennai → Egmore → the plot → tower rises in
timelapse with crews and cranes → dusk rooftop reveal.

**Answer up front: yes, but not as one generation.**

Veo caps at **8 seconds per clip** and will not hold a specific real building's
identity through a long moving-camera shot. That journey is 45–60s. Anyone who
tells you one prompt does this is selling you a slot machine.

What it *is* is **six shots, 8s each, cut on matched motion** so it plays as one
continuous flight. That's how every Google-Earth-zoom ad you've ever seen is
actually made. And the first third of it shouldn't come from an AI model at all.

---

## First — you have two different buildings. Pick one.

| Asset | Design |
|---|---|
| `RWD Corniche.jpg` (render) | Dark charcoal facade, **solid** cantilevered canopies with lit timber soffits, faceted glass lift shaft |
| `Terrace Dusk a - Copy.jpg` (render) | Same family — solid slab canopies, same massing |
| `images.jpeg` (**real photo, as built**) | Cream/white facade, amber vertical LED strips, **open rectangular pergola** crowns |

The as-built tower is not the render. Your chosen ending is `Terrace Dusk`,
which belongs to the **render family** — so build the whole film in the render
family and use `images.jpeg` as reference only. If you mix them, the tower
changes design halfway through the film and a buyer standing in front of the
real building will notice.

*(Correction to what I told you earlier: `PROMPTS.md` wasn't describing the
wrong building — it was accurately describing `images.jpeg`, the as-built. I was
looking at the render. Both files are real; they're just different designs.)*

---

# The six shots

Format **16:9, 1080p** throughout. Veo audio **off** on every clip — score it in
edit.

---

## Shot A — Space to Egmore (12s) · **Google Earth Studio, not Veo**

Do not ask Veo to do this. **Google Earth Studio** is free, browser-based, uses
Google's actual satellite and 3D city data, and does exactly this move as its
headline feature. It will look photoreal because it *is* photographs. Veo would
hallucinate a fictional coastline.

1. `earth.google.com/studio` → sign in with a Google account → **Blank Project**,
   1920×1080, 30fps, 12 seconds.
2. Use the **Quick Start → "Point to Point"** preset.
3. **Start camera:** altitude ~11,000,000 m, centred over the Indian Ocean so the
   full globe fills frame, slight tilt.
4. **End camera:** search `Pantheon Road, Egmore, Chennai` in the search bar,
   drop the pin on the Corniche plot, then set altitude ~400 m with camera tilt
   ~55° and a slow clockwise rotation. Verify the pin against Google Maps
   before you commit — don't trust the first search hit.
5. Add mid keyframes so the descent decelerates near the end rather than
   slamming in — Earth Studio's default ease is too abrupt for this.
6. **Render → Image Sequence (JPEG)**, then assemble:
   `ffmpeg -framerate 30 -i Shot_A/%04d.jpeg -c:v libx264 -crf 16 -pix_fmt yuv420p shotA.mp4`
7. Grade it warm in edit so it matches the dawn of Shot B.

**Save the last frame as `shotA_last.jpg`** — it's the input for Shot B.

> Earth Studio's imagery of Egmore is a few years old, so the plot may already
> show the finished tower. That's fine — Shot B replaces it with bare ground and
> the cut hides the swap.

---

## Shot B — Handoff: satellite look → cinematic drone over the bare plot (8s)

Veo 3.1, **image-to-video**, input = `shotA_last.jpg`.

```
Aerial drone shot descending toward a cleared urban construction plot in
Egmore, Chennai, India. The image resolves from flat satellite imagery into
sharp cinematic drone footage as the camera drops. The plot is bare levelled
red-brown earth behind blue site hoarding, with a site office cabin, stacked
steel and one yellow tower crane. Dense low-rise Chennai neighbourhood packed
around it, palm trees, a busy road along one edge with autorickshaws and
traffic. 5:30am dawn, low warm orange sun, soft haze. Smooth continuous
descent, gentle clockwise drift. Photoreal aerial cinematography, shot on a
DJI Inspire, shallow atmospheric depth.
```

---

## Shot C — Excavation and foundation, workers on site (8s)

Veo 3.1, image-to-video, input = last frame of Shot B.

```
Aerial construction timelapse, drone slowly orbiting clockwise around the site
and rising. Excavators dig out a deep pit, dumper trucks come and go, dozens of
Indian construction workers in yellow and blue hard hats lay a dense grid of
steel reinforcement bars across the foundation, concrete pumps pour the raft
slab. Workers move at fast timelapse speed, machinery at natural speed. Clouds
streak overhead, shadows sweep across the site as the sun climbs from dawn to
mid-morning. Photoreal aerial cinematography, sharp, high detail.
```

---

## Shot D — The tower rises (8s) · the centrepiece

Veo 3.1, image-to-video, input = last frame of Shot C.

```
Aerial construction timelapse, drone rising steadily while orbiting the
building clockwise. A residential tower grows floor by floor from the ground
up: bare grey concrete columns and flat slabs stack upward, plywood formwork
and props on each new deck, red blockwork infill walls fill in below, green
safety netting and scaffolding climb the facade, and a yellow tower crane
climbs alongside swinging loads. Indian construction workers in hard hats move
across every deck at fast timelapse speed. The tower reaches roughly fifteen
storeys, two tall wings meeting at a corner. Sunlight sweeps from morning to
harsh midday, clouds race overhead. Photoreal aerial cinematography, sharp,
high detail.
```

---

## Shot E — Facade and finishing, day into night (8s)

Veo 3.1, **frames-to-video**: first frame = last frame of Shot D, **last frame =
`SHOT_F_ending_16x9.jpg`** *(I made this — 1920×1080 crop of your Terrace Dusk
render)*. Giving Veo both endpoints is what forces it to land on your building
instead of inventing one.

```
Timelapse. Scaffolding and green netting come down, the tower crane is
dismantled, facade panels and glazing are installed from the top down, the
cantilevered roof canopies are craned into place, rooftop gardens and the
netted rooftop sports court are laid out, palms and landscaping planted at the
base. Daylight falls through golden hour into deep blue dusk, apartment windows
switch on one by one, facade and rooftop lighting fades up. Drone continues to
rise and orbit. Photoreal aerial cinematography.
```

---

## Shot F — Final hero hold (8s)

Veo 3.1, image-to-video, input = `SHOT_F_ending_16x9.jpg`.

```
Slow cinematic drone move over a fully lit residential tower rooftop at dusk.
The camera drifts gently forward and rises. Players move on the netted rooftop
football court under floodlights, residents sit at the rooftop lounge, trees on
the terrace gardens sway slightly, car light trails streak along the roads
below, apartment windows glow warm. Camera motion slow and smooth, no cuts.
Photoreal aerial cinematography.
```

Hold the last second on a freeze for the logo card.

---

## Negative prompt — use on every Veo shot

```
morphing, melting geometry, warped windows, rubbery structure, changing
building proportions, different building, building shrinking or growing
sideways, glass skyscraper, western downtown skyline, Manhattan, Dubai, added
towers, snow, cartoon, illustration, video game, CGI look, oversaturated,
text, watermark, logo, subtitles, timestamp, distorted faces, extra limbs,
fast camera shake, whip pan
```

---

## Assembling it

```bash
# after picking your best take of each shot
printf "file 'shotA.mp4'\nfile 'shotB.mp4'\nfile 'shotC.mp4'\nfile 'shotD.mp4'\nfile 'shotE.mp4'\nfile 'shotF.mp4'\n" > list.txt
ffmpeg -f concat -safe 0 -i list.txt -c:v libx264 -crf 18 -pix_fmt yuv420p -an corniche_film.mp4
```

Then in your editor: 6–10 frame cross-dissolves at each join (not hard cuts —
the camera speed differs slightly between clips and a dissolve absorbs it), one
music bed, and a light warm-to-cool grade across the whole thing so the sun
genuinely appears to travel from dawn to night.

Total runtime ~52s.

---

## Realistic expectations

- **Budget 4–6 takes per shot.** Veo is a slot machine; the re-roll is cheap and
  the variance is large. Shot D is the hard one — expect the most attempts.
- **Continuity between shots will not be perfect.** Neighbouring buildings will
  shift between clips. Nobody watching notices at this pace; you will, because
  you made it. Don't chase it.
- **Workers will have bad hands and faces at close range.** Keep the drone high
  enough that people are 20–40 px tall and it's a non-issue. That's why every
  shot here is aerial.
- **Shot A is the one that sells it** and it's the one with zero AI risk,
  because it's real satellite data. Spend your time there.

---

## Before delivery

- Confirm with RWD you can use their renders as generation inputs.
- Check Google's current commercial-use terms for your AI Studio tier. Veo
  output carries SynthID watermarking.
- Google Earth Studio output requires the **Google attribution** shown in the
  render to stay visible, and is licensed for non-commercial / editorial use by
  default — **read their terms before this goes into a paid client deliverable.**
  This is the one genuine legal snag in the plan; check it early, not the night
  before delivery.
