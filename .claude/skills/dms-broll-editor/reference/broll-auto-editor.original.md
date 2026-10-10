---
name: broll-auto-editor
description: Auto-edit a talking-head video with full B-roll coverage — transcribe it, match clips from the user's own B-roll library to the script, review a shot list, render with B-roll muted over the talking-head audio. Use for any "add B-roll to my video" request.
---

# B-roll auto editor

Turns a talking-head video into a fully B-rolled vertical reel. Every second is covered by B-roll, all B-roll audio is muted, and only the talking-head audio plays.

## Step 0 — Intake (always do this first)

Never assume where the media is. Work out four things before touching a file.

**1. The talking-head video.** Say: *"Drop your talking-head video in here, or tell me the folder it's in on your computer."* Two ways it can arrive:

- Dragged into the chat — fine, as long as it really is the video. **A video dragged out of Apple Photos arrives as a single still frame.** If what lands is one image, say so and ask them to save the file to a folder first (in Photos: File → Export → Export Unmodified Original) and connect that folder.
- Already on their computer — have them connect the folder with "Add folder" in the desktop app, then read it from there. This is the better path: no upload wait, no size cap.

**2. Their B-roll.** Ask where their clips live and have them connect that folder. If their library is in Apple Photos rather than a folder, they need an export first — one folder per album works well, and an Automator workflow can keep it in sync weekly. Without a connected folder there is no B-roll to pull from, so settle this before going further.

**3. The edit rules.** Ask these together, with defaults offered, and skip any the user already stated:

- Cut pace — default every 3–5 seconds
- Faces and privacy — anyone whose face should never appear (kids especially): skip those clips, or blur the face when the clip is the best match
- Format — default 1080x1920 vertical for Reels/TikTok; ask if they post elsewhere
- Anything off limits in their library — albums or clips not to touch

**4. First-run cost.** Cataloging the library takes roughly 5 seconds per clip, so a 400-clip library runs about half an hour. Tell them the number up front and offer to start with the albums most likely to fit this script. The catalog is cached, so later videos are fast.

Save what they answer so a returning user is not asked twice, and note where their folders are.

## The rules that define a good edit

- **Cuts every 3–5s on average.** Shorter or longer is fine when the line calls for it. Cut on phrase boundaries (the gap between words), never mid-word.
- **Match the clip to what they're saying.** When nothing matches, pick a clip that fits the mood rather than forcing it. Never repeat a clip within ~10 shots.
- **Use the moving part of a clip.** Pick the window with the most motion, not a static hold.
- **Protected faces never appear.** Prefer clips where the face isn't shown (backs, helmets, distance). If the best clip shows one, blur it and say so. Always sample frames across the whole chosen window before calling a clip safe — a face can turn toward camera halfway through.
- **Avoid clips of them talking to camera** where the muted lips will fight the audio, unless the shot is short or the point is the gesture.

## Process

### 1. Transcribe the talking-head video

Stage it into the cloud workspace and run the `text-video-editor` skill's `scripts/transcribe.py`. It gives a `.transcript.md` and a `.transcript.json` with word-level timings. Expect a few mis-heard words ("paid in full" → "painful"); they affect the text only, not the timing.

### 2. Catalog the B-roll (once, then incrementally)

On their machine (`device_bash`), for each clip: probe duration and size, decode a downscaled grayscale copy at ~5fps, compute frame-to-frame motion, keep the highest-motion 4s window, and save 3 thumbnails from it. Cache one JSONL record per clip (`id`, `path`, `album`, `dur`, `best_t`, `best_motion`, `thumbs`) and skip clips already recorded, so later runs only process new files. Build per-album contact sheets (20 clips per sheet, 3 thumbs each, labeled with id / duration / motion), write them into the working folder, stage them, and Read them to learn what each clip actually shows.

### 3. Build the shot list

Assign a clip to each beat of the script. Anchor every cut to a phrase from the transcript (find the word sequence, cut at the midpoint of the gap before it) rather than to a raw timestamp. Choose the source in-point from `best_t`, centered if the shot is shorter than the motion window. Check the average shot length and merge or split until it lands near 3–5s.

### 4. Review before rendering

Render one frame per shot **through the final crop** so they see the real framing, tile the frames into one image, and slice it into an HTML review page: thumbnail, timecode, the words that shot covers, why the clip was picked, album and clip id, plus a note on any shot with a protected face in it. Send it with SendUserFile and wait for a go-ahead. Call out picks they may want to swap and anything sensitive.

### 5. Render

One segment file per shot, so a later swap re-renders only that shot:

- `scale=W:H:force_original_aspect_ratio=increase,crop=W:H,fps=30,setsar=1,format=yuv420p`, libx264, crf 18, `-an`
- Frame-exact: `-frames:v round(end*30) - round(start*30)`; verify each segment's frame count before concatenating
- HDR clips (`color_transfer=arib-std-b67`) need a zscale/tonemap chain first, or they render washed out
- Concat with the concat demuxer and `-c:v copy`, then map audio from the original talking-head file (`-map 0:v:0 -map 1:a:0`)

Verify by pulling frames from the finished file and looking at them.

### 6. Deliver both copies

- Full quality into the working folder as `<NAME>_broll_edit.mp4`
- A phone copy beside it (`_phone.mp4`, ~1500k video / 96k audio, same resolution) and send that one with SendUserFile — the chat rejects anything over 30MB

## Gotchas worth remembering

- **Each `device_bash` call is its own process namespace.** Background jobs die when the call returns, and calls cap around 175s. Split long work into resumable passes with a cache file, and never `pkill -f` a pattern that matches the running command itself.
- **`ffprobe`/`ffmpeg` eat stdin in shell loops.** Always pass `stdin=DEVNULL` (or `< /dev/null`).
- **A clip they send may not be in the library.** Comparing a still against cached thumbnails is a weak signal; confirm by looking at frames from the candidate, and if it's genuinely absent, ask them to drop it into the right album.
- **Speech models need a download.** If Hugging Face is blocked, the text-video-editor skill falls back to a GitHub-hosted model; if both are blocked, ask for an existing .srt from CapCut, Descript or Instagram.
- **Keep scratch files off their folders** except the ones they'll want (contact sheets, storyboard, checks, final videos). Deleting files on their machine needs their permission, so don't litter.
