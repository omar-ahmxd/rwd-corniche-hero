# Generating the clips

## Settings

| Setting | Value | Why |
|---|---|---|
| Resolution | **1080p** (not 4K) | Anchors are 2752px; 4K adds nothing and costs several times the credits |
| Aspect | 16:9 | Anchors are already 2752x1536 |
| Length | 5s per clip | 7 clips x 5s = ~850 frames; you need ~250 total, so sample down |
| Output | MP4, extract to PNG immediately | Local PNG output is gone; extract before any re-encode |
| Cost | 40 credits per clip (Kling Standard) | 7 clips = 280 |

## Tool

**Kling 3.0**, VIDEO 3.0, Frames mode (start + end), 16:9, 1 output, Native Audio off.
No negative-prompt field exists in 3.0 — the negatives are inlined into the prompts as
positive statements. Local generation and Google Flow were both tried and rejected; see
`CLAUDE.md`.

Use the anchors in `sequence/sequence-clean/` — the ones in `sequence/` carry a Gemini
sparkle watermark that propagates into every clip.

## Order

Run the locked-off day-to-night clip first (`05-complete-day` -> `06-complete-dusk`). Locked-off camera, both anchors are
true renders, nothing else can go wrong. It answers the only question that matters: is the end
frame actually being honoured?

Then generate in story order — C1, C2, C3, C4, C6, C7. Each clip inherits drift from the one
before, and clips C4, C5 and C7 all end on genuine anchors, so those are your correction points.

## What to check

One question per clip: **does the last frame match the end anchor?**

- Tower same width, same height, same roof profile
- Windows in the same positions
- Building has not rotated or shifted on the plot

Ignore whether it looks pretty. A beautiful clip that ends on a different building is a
failure. An ugly one that lands exactly on the anchor is a pass.

Compare properly rather than by eye — pull the last frame and diff it:

```bash
ffmpeg -sseof -0.1 -i clip.mp4 -update 1 -q:v 1 last.png
```

If you generated PNG sequences directly, just open the final file.

## Chaining beyond one clip

If a clip is too short for the span, chain it: the last frame of clip N becomes the first frame
of clip N+1. Because both ends stay locked, drift cannot accumulate the way it does with plain
image-to-video.

## Known issues in the current anchor set

Read `sequence/_clips.txt`. Summary:

- **04** has a stray excavation pit at the tower base. Wrong for a topped-out stage. One edit
  pass fixes it — prompt is in `_clips.txt`.
- **Crane jumps left to right between 03 and 04.** Massing and bay grid match, which matters
  more; at time-lapse speed a relocated crane may not read.
- **C8 has no end anchor.** A real orbit needs 3-4 rotational plates descending around the
  tower, each generated from the previous — not one viewpoint.
- **01-04 are a generic tower**, not Corniche. Fine for proving the pipeline. Before anything
  reaches the client, rebuild the chain from `anchors/A-day.jpg` by subtraction:
  `complete -> unclad -> 2/3 -> 1/3 -> foundation -> empty`. Each step edits the previous
  image and removes something. Never generate a stage fresh — that is how footprints drift.

## Next

`docs/03-frames-to-web.md`
