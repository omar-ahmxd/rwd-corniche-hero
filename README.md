# RWD Corniche — Scroll Hero

A scroll-driven construction-to-night hero for RWD Corniche, Pantheon Road, Egmore, Chennai.
The user scrolls; the tower builds itself, day turns to night, and the camera lifts into a
rooftop orbit. Scroll position is the timeline — there is no video element.

## The pipeline

```
anchor stills  ->  Kling 3.0     ->  MP4  ->  PNG frames  ->  sample  ->  upscale  ->  WebP  ->  canvas
   (this repo)     (start+end)       1080p    (ffmpeg)      (~250)     (detail)   (~1400px)  (web/)
```

The whole design rests on one idea: **every clip is defined by two images, a start frame and an
end frame.** The model only invents the motion between them, so it cannot redraw the building.
Chain the clips — the end frame of one is the start frame of the next — and there are no seams
to hide.

This is now measured rather than assumed. A clip passes when its last frame diffs under
~15/255 against the end anchor. See `CLAUDE.md`.

## Why Kling

Local generation was the original plan and every local model attempt failed. Google Flow was
tested and rejected: Veo 3.1 on the AI Pro tier is 720p with a visible watermark burned into
every frame, and both 1080p and watermark removal are gated behind the ~Rs 6,500/mo Ultra plan.

Kling 3.0 Standard, at $6.99 for the first month, gives 1080p, brand watermark removal and
commercial use. On the same anchors and the same prompt it also beat Veo on fidelity:

|                        | Veo 3.1 (Flow) | Kling 3.0     |
|------------------------|----------------|---------------|
| resolution             | 1280x720       | 1928x1072     |
| first frame vs anchor  | 14.96 / 255    | 9.09 / 255    |
| last frame vs anchor   |  9.68 / 255    | 7.02 / 255    |

Both test clips are in `clips/`.

## Repo map

| Path | What |
|---|---|
| `sequence/` | The 8 anchor stills, in scroll order. The whole film is defined here. |
| `sequence/_clips.txt` | Clip pairings, known issues, rejected plates. |
| `anchors/` | Source renders pulled from the 2017 client brochure. |
| `clips/` | Generated video clips. Frames are extracted from these, never committed. |
| `prompts/clips.md` | Every clip prompt, paste-ready. Motion only. |
| `prompts/anchors.md` | Anchor still prompts — master plate, subtraction, relighting. |
| `docs/01-setup-5090.md` | Obsolete — local generation was abandoned. Kept for history. |
| `docs/02-generate.md` | Generation settings and what to check. |
| `docs/03-frames-to-web.md` | Frames -> RIFE -> upscale -> WebP -> the hero. |
| `web/corniche-hero.html` | Working canvas rig. Open it in a browser. |

## Quick start

Read `CLAUDE.md` first — it carries every decision and the reasoning. Then:

1. Generate a clip in Kling 3.0: VIDEO 3.0, **1080p**, 5s, 16:9, 1 output, Native Audio off.
   Start and end frames from `sequence/sequence-clean/`. Prompt from `prompts/clips.md`.
2. Extract frames: `ffmpeg -i clip.mp4 -vsync 0 -q:v 1 frames/x_%04d.png`
3. Run the diff in `CLAUDE.md`. Under ~15/255 passes.
4. Then `docs/03-frames-to-web.md`.

## Status

- Anchors: eight unwatermarked plates in `sequence/sequence-clean/`
- Clips generated: 1 of 7 (C4, as the method test — it passed)
- Canvas rig: built, running on placeholder plates

## Known limitations

Read `sequence/_clips.txt` and the open issues in `CLAUDE.md` before generating.

The big one: **the tower in these anchors is not RWD Corniche and the city is not Egmore.** It
is a generic tower in a generic masterplan, accepted deliberately for its archviz quality while
the pipeline is proven. Rebuild from `gen2.jpeg` by subtraction before anything reaches the
client — `prompts/anchors.md` has the prompts and `anchors/D-asbuilt-front.jpg` shows what the
building actually looks like.

Also: `04-frame-topped` still has a stray excavation pit at the tower base, and the rooftop
orbit has no end anchor.
