# Frames to the web

## 1. Extract, if you ended up with video

Generating PNG sequences directly is better. If you have an MP4, pull frames without
re-encoding:

```bash
ffmpeg -i clip.mp4 -vsync 0 -q:v 1 out/%04d.png
```

`-vsync 0` keeps every frame at source rate. `-q:v 1` avoids a second compression pass.

## 2. Smooth it — RIFE

Frame interpolation decouples smoothness from generation cost. Generate fewer frames, then
interpolate up. Free, local, and it means a short clip still scrolls like butter.

Use RIFE via ComfyUI (`ComfyUI-Frame-Interpolation`) or Flowframes. 2x is usually enough.

## 3. Upscale — after extraction, never before

**Upscayl** (free, open source, local) on the PNG frames. Working on uncompressed source is why
this recovers real detail. Target ~1400px wide.

## 4. Renumber into one continuous sequence

Eight clips become one film. Target **200-300 frames total** — at 24fps that is 10-12 seconds of
footage, so subsample rather than keeping everything:

```bash
ffmpeg -i clip.mp4 -vf fps=12 -vsync 0 out/%04d.png
```

Then renumber across all clips into a single `frame-0001.webp ... frame-0250.webp` run.

## 5. Convert to WebP

```bash
for f in *.png; do cwebp -q 80 "$f" -o "${f%.png}.webp"; done
```

Payload is your real ceiling, not the model:

| Frame width | Each | 250 frames |
|---|---|---|
| 1920 | ~150-200 KB | 38-50 MB |
| 1600 | ~100-140 KB | 25-35 MB |
| **1400** | **~60-90 KB** | **15-22 MB** |

Two things work in your favour: the sharp plate is not full-bleed (the building sits centred
with a blurred extension filling the sides, so the crisp region is the middle ~60% of the
viewport), and half the sequence is night — dark low-contrast frames compress far better.

Do not chase 2x for retina. Nobody ships a 2x frame sequence; the payload kills it, and motion
covers what resolution would have.

## 6. Drop into the rig

`web/corniche-hero.html` currently cross-dissolves two stills. The renderer already takes an
array of plates and a scroll position, so swapping in frames is a change of input, not
architecture — the pin, the scrub, the device-pixel-ratio handling and the reduced-motion
fallback all stay as they are.

```js
// today - two plates, cross-dissolved
const plates = [ day, dusk ];

// once frames exist
const plates = await preload(
  250, i => `frames/frame-${String(i).padStart(4,'0')}.webp`
);
```

Then `draw(progress)` picks the nearest frame instead of blending two.

## 7. Loading

For a hero the user sees immediately, block on a loading state until frames are ready. Lazy
loading mid-scroll stutters, and a stutter in the first three seconds costs more than the wait.
