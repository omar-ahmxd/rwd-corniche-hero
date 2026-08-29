# RWD Corniche — Scroll Hero

A scroll-driven construction-to-night hero for RWD Corniche, Pantheon Road, Egmore, Chennai.
The user scrolls; the tower builds itself, day turns to night, and the camera lifts into a
rooftop orbit. Scroll position is the timeline — there is no video element.

## The pipeline

```
anchor stills  ->  Wan 2.2 FLF2V  ->  PNG frames  ->  RIFE  ->  upscale  ->  WebP  ->  canvas
   (this repo)     (start+end)       (no MP4)      (smooth)   (detail)    (~1400px)  (web/)
```

The whole design rests on one idea: **every clip is defined by two images, a start frame and an
end frame.** The model only invents the motion between them, so it cannot redraw the building.
Chain the clips — the end frame of one is the start frame of the next — and there are no seams
to hide.

## Why local, not a cloud tool

Cloud generators hand you a compressed MP4 that you then extract frames from. That bakes in
compression before the pipeline even starts, which is the exact fault this project exists to
avoid. ComfyUI writes PNG sequences directly — no video file is ever created.

It is also free, unlimited, watermark-free, and Apache 2.0, which settles the commercial
licensing question for a client deliverable.

## Repo map

| Path | What |
|---|---|
| `sequence/` | The 8 anchor stills, in scroll order. The whole film is defined here. |
| `sequence/_clips.txt` | Clip pairings, known issues, rejected plates. |
| `anchors/` | Source renders pulled from the 2017 client brochure. |
| `prompts/clips.md` | Every clip prompt, paste-ready. |
| `docs/01-setup-5090.md` | CUDA, ComfyUI, Wan 2.2 on a 5090. Start here. |
| `docs/02-generate.md` | Generation settings and what to check. |
| `docs/03-frames-to-web.md` | Frames -> RIFE -> upscale -> WebP -> the hero. |
| `web/corniche-hero.html` | Working canvas rig. Open it in a browser. |

## Quick start on the 5090

```bash
git clone <this repo>
cd rwd-corniche-hero
```

Then work through `docs/01-setup-5090.md`. Blackwell needs a current CUDA stack — that is the
one thing that will stop you, and it is covered first.

## Status

- Anchors 01-08: complete, one consistent camera and city
- Clips generated: 0 of 8
- Canvas rig: built, running on placeholder plates

## Known limitations

Read `sequence/_clips.txt` before generating. In short: `04` still has a stray excavation pit,
the crane jumps between `03` and `04`, the orbit has no end anchor yet, and plates `01-04` are a
generic tower rather than Corniche itself — fine for proving the pipeline, not for delivery.
