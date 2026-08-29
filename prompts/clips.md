# Clip prompts

**Prompt the motion only.** The two anchor images already define the building completely.
Describing it again gives the model permission to reinterpret it — which is the exact failure
this whole approach exists to prevent. No "modern tower", no "timber louvers", no
"photorealistic architectural visualization". Motion and light, nothing else.

Every clip: **16:9**, single continuous motion, no cuts, no text.

---

## C1 — 01-site-empty -> 02-foundation

> Accelerated construction time-lapse. Locked-off camera, no movement. Ground is excavated,
> foundations poured, tower cranes erected. Clouds drift. Smooth continuous motion, no cuts,
> no text.

## C2 — 02-foundation -> 03-frame-mid

> Accelerated construction time-lapse. Camera rises slowly. Bare concrete floors stack upward
> one after another. Tower crane swings. Structure only — no facade, no glass, no cladding.
> Consistent daylight throughout. Smooth continuous motion, no cuts, no text.

## C3 — 03-frame-mid -> 04-frame-topped

> Accelerated construction time-lapse. Camera continues rising slowly. Remaining concrete
> floors stack to full height. Structure only — bare grey concrete, no facade, no glass, no
> cladding. Consistent daylight throughout. Smooth continuous motion, no cuts, no text.

## C4 — 04-frame-topped -> 05-complete-day

> Accelerated time-lapse. Camera pulls back slowly. The facade forms across the structure from
> bottom to top — glass first, then timber louvers, then the roof canopy last. Cranes removed.
> Consistent daylight. Smooth continuous motion, no cuts, no text.

## C5 — 05-complete-day -> 06-complete-dusk

> Time-lapse. Locked-off camera, no movement. Sunlight shifts through golden hour into dusk.
> Shadows lengthen, clouds drift, windows begin to glow. Smooth continuous motion, no cuts,
> no text.

**Run this one first.** Locked-off camera, both anchors are true renders, lowest risk in the
set. It tests the method, not the art.

## C6 — 06-complete-dusk -> 07-complete-night

> Time-lapse. Locked-off camera, no movement. Sky darkens to night. Apartment windows
> illuminate progressively floor by floor. Streetlights and landscape lighting come on.
> Smooth continuous motion, no cuts, no text.

## C7 — 07-complete-night -> 08-aerial-night

> Aerial drone shot. Camera rises smoothly from street level and tilts down toward the rooftop.
> Single unbroken camera motion, no cuts, no text.

## C8 — 08-aerial-night -> (end anchor not yet made)

> Aerial drone shot. Camera orbits slowly around the rooftop while descending around the tower.
> Single unbroken camera motion, no cuts, no text.

---

## Two prompt phrases that carry weight

**"Structure only — no facade, no glass, no cladding."** In C2 and C3 the end anchor is bare
frame. Without this the model knows "finished building" is where construction goes, starts
cladding early, then has to strip it back to hit the anchor. That is what makes frames flicker.

**"Consistent daylight throughout."** Both anchors in C2, C3 and C4 are daytime. Left alone,
video models drift toward golden hour because that is what archviz footage looks like in
training data.

## What was tried and failed

**01 -> 07 in a single clip.** It dissolves — the tower fades up out of the ground instead of
building. The span is too long for one generation. Split into 01->04 and 04->07 if you want to
compress the sequence.
