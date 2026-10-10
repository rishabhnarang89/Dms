---
name: dms-broll-editor
description: Auto-edit a talking-head video with full B-roll coverage — transcribe it, match clips from the user's own B-roll library to the script, review a shot list, render with B-roll muted over the talking-head audio. Use for any "add B-roll to my video" request. Doodles Matter Studio (DMS) edition — use for any DMS Instagram Reel built from a talking-head video or a voice memo plus the DMS B-roll library, e.g. "make a DMS reel", "B-roll this for DMS", "here's a voice note and some clips for the studio page".
---

# B-roll auto editor

> **Doodles Matter Studio edition.** This is the original `broll-auto-editor` skill with DMS-specific additions layered on top. Every line of the original is kept, in its original order (an untouched copy is in `reference/broll-auto-editor.original.md`). Additions are marked **DMS**. Where a DMS default differs from an original default, the DMS one applies to DMS reels and the original stays as the general rule.

Turns a talking-head video into a fully B-rolled vertical reel. Every second is covered by B-roll, all B-roll audio is muted, and only the talking-head audio plays.

## DMS profile (read before Step 0)

- **Brand:** Doodles Matter Studio — Instagram page. Handle: `@TODO` (ask once, then fill in here and in `profile.md`).
- **What the page posts** (assumed; confirm on first run and correct this list):
  1. Tips & techniques — short, punchy, "3 ways to…", "stop doing…"
  2. Process / behind the scenes — a piece from blank page to finished
  3. Finished pieces & commissions — the story behind a piece
  4. Workshops & classes — what happens in a session, announcements
  5. Why doodling matters — mission, opinion, the studio's point of view
- **Folders** (suggested layout; use whatever the user already has and record it):
  - B-roll library: `DMS-Broll/` with one sub-folder per album (album map below)
  - One working folder per reel: `DMS-Reels/<YYYY-MM-DD>-<slug>/`
  - Editor state: `DMS-Reels/_editor/` → `profile.md` (Step 0 answers, consent list, shot wishlist), `catalog.jsonl` (clip catalog), `contact-sheets/`
- Read `DMS-Reels/_editor/profile.md` before asking anything in Step 0. Only ask what's missing.

### DMS album map

Albums are a starting hint for matching. The contact sheets are the truth: always look at the frames before picking.

| Album | What's in it | Spoken cues that point here |
|---|---|---|
| `hands-drawing-closeup` | pen tip on paper, lines being drawn, shading, hatching, fixing a mistake | draw, line, stroke, sketch, shade, practise, wobbly, mistake, fix |
| `pens-markers-materials` | fineliners, markers, brushes, paper, washi tape, colour swatches | pen, fineliner, marker, brush, paper, tools, supplies, colour |
| `sketchbook-flips` | flipping pages, old vs new pages | sketchbook, page, every day, habit, progress, months, years |
| `finished-pieces` | finished work, slow push-ins, a hand holding or turning the piece | finished, final, result, piece, artwork, reveal, done |
| `studio-space` | desk, shelves, window light, wide studio shots | studio, space, desk, where I work, setup, morning |
| `timelapses` | sped-up full drawings | hours, whole process, start to finish, build-up |
| `workshops-classes` | sessions, group tables, students' hands at work | workshop, class, students, kids, together, join, learn |
| `packing-shipping` | wrapping prints, orders, parcels going out | order, shop, print, ship, commission, deliver |
| `misc` | anything that doesn't fit yet | mood fallback only |

Albums named `_private` or `_do-not-use` are never read. Claude may suggest new albums when a pattern shows up in `misc`.

### DMS edit defaults (offer these in Step 0; the user can override)

- **Cut pace:** 3–4s for story, process and mission reels; 2–3s for tips reels.
- **Format:** 1080x1920, 30fps. Keep the action out of the bottom ~20% and top ~12% of the frame, where Instagram's caption, buttons and username cover it. Check this on the review frames.
- **Faces:** every student and every child is protected by default. Only people on the consent list in `profile.md` may be shown. The owner's own face is allowed, but the "muted lips" rule below still applies.
- **Off limits by default:** commissions and client work until the user says they're public; the `_private` / `_do-not-use` albums.
- **Opening shot:** the strongest, most active clip that matches the hook line. The first 1–2 seconds decide whether people keep watching.
- **Closing shot:** a finished piece or a wide studio shot, so the reel ends on the work (this also helps it loop and supports the CTA).

## Step 0 — Intake (always do this first)

Never assume where the media is. Work out four things before touching a file.

**1. The talking-head video.** Say: *"Drop your talking-head video in here, or tell me the folder it's in on your computer."* Two ways it can arrive:

- Dragged into the chat — fine, as long as it really is the video. **A video dragged out of Apple Photos arrives as a single still frame.** If what lands is one image, say so and ask them to save the file to a folder first (in Photos: File → Export → Export Unmodified Original) and connect that folder.
- Already on their computer — have them connect the folder with "Add folder" in the desktop app, then read it from there. This is the better path: no upload wait, no size cap.

**DMS — a voice memo is enough.** Because every second of the reel is covered by B-roll, the talking-head picture is never seen; only its audio is used. A phone voice memo (`.m4a`, `.wav`) works exactly like a talking-head video: it transcribes the same way, and the render step's `-map 1:a:0` takes its audio. Tell the user this up front: *"No need to film yourself — a voice note is enough."*

**DMS — no script yet?** If the user has a topic but no script, write a 25–45s script (about 70–110 words) for them to read:

- Hook in the first line (a claim, a mistake, a surprising result)
- Concrete nouns the library can show ("a 0.3 fineliner", "the first page of my sketchbook") rather than abstract ones
- One idea per sentence, so each sentence becomes one or two shots
- End with one CTA (follow, save, join the next workshop)

Before handing it over, check every sentence against the catalog (or the album map, if there's no catalog yet). Mark lines with no matching footage so the user can reword them or film that one shot.

**2. Their B-roll.** Ask where their clips live and have them connect that folder. If their library is in Apple Photos rather than a folder, they need an export first — one folder per album works well, and an Automator workflow can keep it in sync weekly. Without a connected folder there is no B-roll to pull from, so settle this before going further.

**DMS:** suggest the album layout above if they don't have folders yet. Their already-posted Reels and Stories count as B-roll: downloading the originals from their phone or camera roll gives them a library on day one.

**3. The edit rules.** Ask these together, with defaults offered, and skip any the user already stated:

- Cut pace — default every 3–5 seconds
- Faces and privacy — anyone whose face should never appear (kids especially): skip those clips, or blur the face when the clip is the best match
- Format — default 1080x1920 vertical for Reels/TikTok; ask if they post elsewhere
- Anything off limits in their library — albums or clips not to touch

**DMS:** offer the DMS edit defaults above instead of the generic ones, and ask for the consent list (who may appear on camera).

**4. First-run cost.** Cataloging the library takes roughly 5 seconds per clip, so a 400-clip library runs about half an hour. Tell them the number up front and offer to start with the albums most likely to fit this script. The catalog is cached, so later videos are fast.

Save what they answer so a returning user is not asked twice, and note where their folders are.

**DMS:** save it to `DMS-Reels/_editor/profile.md` with these headings: Folders, Edit defaults, Consent list, Off limits, Handle, Shot wishlist.

## The rules that define a good edit

- **Cuts every 3–5s on average.** Shorter or longer is fine when the line calls for it. Cut on phrase boundaries (the gap between words), never mid-word.
- **Match the clip to what they're saying.** When nothing matches, pick a clip that fits the mood rather than forcing it. Never repeat a clip within ~10 shots.
- **Use the moving part of a clip.** Pick the window with the most motion, not a static hold.
- **Protected faces never appear.** Prefer clips where the face isn't shown (backs, helmets, distance). If the best clip shows one, blur it and say so. Always sample frames across the whole chosen window before calling a clip safe — a face can turn toward camera halfway through.
- **Avoid clips of them talking to camera** where the muted lips will fight the audio, unless the shot is short or the point is the gesture.

**DMS rules on top:**

- **Show the line being made.** In `hands-drawing-closeup`, use the window where the pen tip is moving across the paper, not where the hand is resting or reaching for a pen. The motion score usually finds this; confirm it on the thumbnails.
- **A named piece means that piece.** When the script names a specific artwork or commission, use clips of that piece only. If the catalog has none, say so; never silently swap in a different piece.
- **No spoilers.** In process stories, don't show the finished piece before the line that reveals it.
- **Keep students anonymous.** No legible names on paper, name tags or school logos, as well as no protected faces. Blur when the clip is the best match, and say so.
- **Vary the albums.** Avoid three clips in a row from the same album unless the script stays on one thing for that long.

## Process

### 1. Transcribe the talking-head video

Stage it into the cloud workspace and run the `text-video-editor` skill's `scripts/transcribe.py`. It gives a `.transcript.md` and a `.transcript.json` with word-level timings. Expect a few mis-heard words ("paid in full" → "painful"); they affect the text only, not the timing.

**DMS fallback, if `text-video-editor` isn't installed:** `pip install faster-whisper` and transcribe with `WhisperModel("small.en")` and `word_timestamps=True`, writing the same two files: `.transcript.json` holds a list of `{word, start, end}`, and `.transcript.md` holds the plain text with a `[mm:ss]` stamp per sentence. If the model can't download, follow the speech-model gotcha below. An `.srt` only gives phrase-level timing, so cut at the gaps between subtitle entries. Art words get misheard ("fineliner" → "fine liner", "washi" → "washy"); fix them in the `.md`, since the timings stay correct.

### 2. Catalog the B-roll (once, then incrementally)

On their machine (`device_bash`), for each clip: probe duration and size, decode a downscaled grayscale copy at ~5fps, compute frame-to-frame motion, keep the highest-motion 4s window, and save 3 thumbnails from it. Cache one JSONL record per clip (`id`, `path`, `album`, `dur`, `best_t`, `best_motion`, `thumbs`) and skip clips already recorded, so later runs only process new files. Build per-album contact sheets (20 clips per sheet, 3 thumbs each, labeled with id / duration / motion), write them into the working folder, stage them, and Read them to learn what each clip actually shows.

**DMS:** keep the catalog at `DMS-Reels/_editor/catalog.jsonl` and the contact sheets in `DMS-Reels/_editor/contact-sheets/`. After reading the contact sheets, add three fields to each record:

- `shows`: one line on what the clip shows
- `piece`: the artwork's name, if it can be identified
- `faces`: `none`, `owner`, `consented`, `student`, `child` or `unknown`

This lets later edits skip protected clips quickly. Still sample the chosen window before using a clip, per the face rule.

### 3. Build the shot list

Assign a clip to each beat of the script. Anchor every cut to a phrase from the transcript (find the word sequence, cut at the midpoint of the gap before it) rather than to a raw timestamp. Choose the source in-point from `best_t`, centered if the shot is shorter than the motion window. Check the average shot length and merge or split until it lands near 3–5s.

**DMS:** write it to `<reel folder>/shotlist.json`, one entry per shot: `n`, `start`, `end`, `anchor_phrase`, `words`, `clip_id`, `in_point`, `album`, `why`, `faces`, `blur` (true/false). Aim for the DMS pace (3–4s, or 2–3s for tips reels), apply the opening and closing shot defaults, and check the DMS rules.

### 4. Review before rendering

Render one frame per shot **through the final crop** so they see the real framing, tile the frames into one image, and slice it into an HTML review page: thumbnail, timecode, the words that shot covers, why the clip was picked, album and clip id, plus a note on any shot with a protected face in it. Send it with SendUserFile and wait for a go-ahead. Call out picks they may want to swap and anything sensitive.

**DMS additions to the review page:**

- Draw the Instagram safe-zone lines (top ~12%, bottom ~20%) on each thumbnail, and flag any shot whose action sits under them.
- Show a "weak match" badge on any shot that fell back to a mood clip, with the words it covers.
- End with a **shot wishlist**: the specific clips that would have made this reel better ("close-up of a brush pen on watercolour paper"). Add them to the wishlist in `profile.md` so the library grows where it's thin.

### 5. Render

One segment file per shot, so a later swap re-renders only that shot:

- `scale=W:H:force_original_aspect_ratio=increase,crop=W:H,fps=30,setsar=1,format=yuv420p`, libx264, crf 18, `-an`
- Frame-exact: `-frames:v round(end*30) - round(start*30)`; verify each segment's frame count before concatenating
- HDR clips (`color_transfer=arib-std-b67`) need a zscale/tonemap chain first, or they render washed out
- Concat with the concat demuxer and `-c:v copy`, then map audio from the original talking-head file (`-map 0:v:0 -map 1:a:0`)

Verify by pulling frames from the finished file and looking at them.

**DMS:**

- Face or name blur: crop to the region, apply `boxblur`, then overlay it back for the shot's duration. Re-check the frames after the blur.
- Don't colour-grade unless asked, even if phone and camera clips look different. Point it out on the review page instead.
- For drawings shot top-down on white paper, check that the crop hasn't cut off the line being drawn. If it has, shift the crop (`crop=W:H:x:y`) toward the pen tip rather than centring.

### 6. Deliver both copies

- Full quality into the working folder as `<NAME>_broll_edit.mp4`
- A phone copy beside it (`_phone.mp4`, ~1500k video / 96k audio, same resolution) and send that one with SendUserFile — the chat rejects anything over 30MB

**DMS extras, in the same folder:**

- `<NAME>.srt`: captions built from the word timings, max ~6 words and ~2.5s per caption, with misheard words corrected. Import it into CapCut or Instagram for on-screen captions; this skill doesn't burn captions in.
- `<NAME>_cover.jpg`: the clearest finished-piece frame, to use as the Reel cover.
- `<NAME>_post.md`: a draft caption (hook line, two or three lines of body, one CTA) for the user to edit. Don't invent facts about the piece or the studio; ask, or leave a `[TODO]`.

Music, text hooks and branding are added by the user afterwards, in CapCut or Instagram.

## Gotchas worth remembering

- **Each `device_bash` call is its own process namespace.** Background jobs die when the call returns, and calls cap around 175s. Split long work into resumable passes with a cache file, and never `pkill -f` a pattern that matches the running command itself.
- **`ffprobe`/`ffmpeg` eat stdin in shell loops.** Always pass `stdin=DEVNULL` (or `< /dev/null`).
- **A clip they send may not be in the library.** Comparing a still against cached thumbnails is a weak signal; confirm by looking at frames from the candidate, and if it's genuinely absent, ask them to drop it into the right album.
- **Speech models need a download.** If Hugging Face is blocked, the text-video-editor skill falls back to a GitHub-hosted model; if both are blocked, ask for an existing .srt from CapCut, Descript or Instagram.
- **Keep scratch files off their folders** except the ones they'll want (contact sheets, storyboard, checks, final videos). Deleting files on their machine needs their permission, so don't litter.

**DMS gotchas:**

- **Timelapses score high-motion everywhere**, so `best_t` means little. Pick the window that ends on the reveal instead.
- **iPad or Procreate screen recordings** are 4:3 or wider. They crop hard to 9:16, so check that the canvas is still readable.
- **Top-down white paper makes phone exposure pump.** A clip that flickers in brightness looks worse muted and cut short; prefer a steadier alternative when one exists.
- **Running in a cloud session (Claude Code on the web) with no `device_bash`:** the user's computer can't be reached. Ask them to upload the voice memo and a small batch of clips (about 20, trimmed to the good part), and run the same commands in the session's own shell. Keep the catalog in the session's working folder, and remind them it disappears when the session ends unless it's committed. Uploads have size caps, so the desktop app with a connected folder remains the better long-term setup.
- **Sample / dry-run reels:** when the user wants to see the method before shooting, follow `docs/sample-plan.md` in the DMS repo. Label every sample output `SAMPLE — not for posting` in the file name and on the review page, especially when it uses a synthetic voice or stand-in footage.
