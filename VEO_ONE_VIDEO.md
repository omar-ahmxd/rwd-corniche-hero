# RWD Corniche — one video, tower rising from scratch (Veo 3.1, AI Studio)

One generation. No frame-by-frame stills.
`AI_STUDIO_GUIDE.md` is the 8-still fallback if this doesn't land.

---

## The trick: generate it backwards, then reverse the file

Veo holds your **input image exactly at frame 0** and drifts progressively away
from it as the clip runs. If you feed it the finished render and ask it to
*rise*, the tower is already there — nothing to build. If you ask it to rise
from a plot, you'd need to generate the plot first and Veo's fidelity to your
building lands at the *weakest* end of the clip.

So: feed it the finished render, prompt a **reverse-demolition** (building
un-builds down to bare earth), then flip the video with one ffmpeg command.

After the flip, your building is the **last frame, pixel-perfect** — the money
shot is a real render, not a Veo guess. All the drift sits at the start, where
it's just dirt and a crane and nobody can tell.

---

## Do this

**1. Upload** `VEO_lastframe_9x16.jpg` — I made it (1080×1920, 9:16, whole tower
in frame, road at the bottom). Veo only outputs 16:9 or 9:16; your render is 4:5
portrait, and 16:9 would decapitate the tower. 9:16 also doubles as the Reels /
Shorts cut, which is what the client will actually ask for next.

**2. AI Studio** → left sidebar **Video** (or Media → Generate video) → model
**Veo 3.1**. Settings:

| | |
|---|---|
| Aspect ratio | **9:16** |
| Resolution | **1080p** |
| Duration | **8s** (the max) |
| Audio / Generate audio | **OFF** — Veo's construction ambience will sound wrong; you want music |
| Input image | `VEO_lastframe_9x16.jpg` as the **first frame** |

**3. Paste this prompt:**

```
Locked-off architectural timelapse, camera completely static on a tripod, no
pan, no tilt, no zoom, no push-in, no parallax. The framing never changes for
a single frame.

Time runs backwards. The finished residential tower un-builds itself, floor by
floor, from the top down. First the two timber-soffit roof canopies lift away
and their warm LED strips go dark. Then the golden perforated jali screen
ribbon and the faceted glazed lift shaft peel away downward, strip by strip,
exposing bare grey concrete beneath. Dark charcoal facade panels, window glass
and the timber podium louvres detach in sequence from the top down, leaving raw
blockwork. Scaffolding and green safety netting climb back up the structure and
a yellow tower crane reappears alongside it. The concrete floor slabs then
disappear one at a time from the top, the tower shrinking steadily downward
until only the foundation raft and a grid of steel reinforcement bars remain,
and finally bare levelled red-brown earth behind blue site hoarding.

Simultaneously the light runs from deep blue dusk backwards through golden
hour, afternoon, harsh midday, morning, to pale dawn. Thin clouds streak fast
across the sky throughout.

The road, the white boundary wall, the street lamps, the trees and the distant
low-rise Chennai skyline stay exactly where they are and never move.

Photoreal architectural cinematography, sharp, high detail, natural colour.
```

**4. Negative prompt** (Veo has this field — use it):

```
camera movement, pan, tilt, zoom, dolly, handheld shake, morphing, melting
geometry, warped windows, rubbery structure, changing building proportions,
building growing wider or narrower, different building, glass skyscraper,
western downtown skyline, added towers, cartoon, illustration, CGI, video game,
oversaturated, text, watermark, logo, subtitles, people close to camera
```

**5. Generate 3–4 takes.** Video models are a slot machine; the cost of a
re-roll is nothing and the variance is large. Pick the take where the tower's
width and the horizon line stay put.

**6. Reverse it:**

```bash
ffmpeg -i veo_backwards.mp4 -vf reverse -an corniche_rising.mp4
```

`-an` strips audio (it'd be reversed and unusable anyway). Done — one video,
tower rising from bare earth to a fully lit dusk hero.

---

## If you want it longer than 8 seconds

Three options, in order of how well they work:

1. **Slow it down.** 8s of un-building reversed and retimed to 12–16s reads as
   a more considered timelapse and hides interpolation wobble:
   `ffmpeg -i corniche_rising.mp4 -vf "setpts=1.75*PTS" -an corniche_rising_slow.mp4`
2. **Veo 3.1 "Extend"** — adds 7s onto an existing clip. Extend the *backwards*
   clip further into the past (more excavation, more dawn), then reverse the
   whole thing. Extending forwards from a finished building gives you nothing.
3. **Two clips, one cut.** Clip A: dusk → topped-out grey shell. Clip B:
   topped-out grey shell → bare earth. Reverse both, join. Only worth it if
   8–16s genuinely isn't enough.

---

## What to check in the output

- [ ] Horizon and road edge do not move — scrub the timeline, they must be nailed
- [ ] Tower doesn't breathe wider/narrower as floors come off
- [ ] Roof canopies, golden jali and glazed lift shaft all disappear *before*
      the floor slabs start going (order matters or it reads as demolition, not
      construction)
- [ ] Crane appears and stays on the same side
- [ ] No text or logos hallucinated onto the hoarding
- [ ] Final frame after reversing = your render, unaltered

---

## Two things to settle before delivery

- **Rights to the source.** Confirm with RWD that you can use their marketing
  render as a generation input. If the render came off a listing site rather
  than from RWD directly, get it in writing.
- **Google's terms.** Commercial-use terms differ between the free and paid
  tiers, and Veo output carries SynthID watermarking. Five minutes on the
  current terms before this goes to the client.
