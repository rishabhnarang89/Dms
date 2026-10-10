# DMS reel editor — seeing a sample before shooting anything

Goal: see what the `dms-broll-editor` skill actually produces before anyone films a full talking-head video.

## What we checked in the cloud session (2026-10-10)

| Piece the skill needs | Status here | Notes |
|---|---|---|
| ffmpeg / ffprobe | ✅ available | Includes zscale + tonemap (HDR clips), boxblur (face blur), drawtext |
| Speech-to-text library | ✅ installs | `faster-whisper` via pip |
| Speech-to-text **model** | ❌ blocked | Hugging Face download returns 403 from the network proxy |
| Synthetic voice | ✅ available | ffmpeg `flite` — robotic, fine for a dry run only |
| Your computer / Photos library | ❌ not reachable | Cloud session; only uploaded files |
| Notion page | ❌ blocked | Network policy |

Transcription can be unblocked in any one of three ways:

- Add `huggingface.co` and `cdn-lfs.huggingface.co` under **Allowed domains** in the environment's network settings.
- Run the skill in the Claude desktop app instead.
- Give an `.srt` file from CapCut or Instagram auto-captions.

The key fact behind the plan: **the talking-head picture never appears in the final reel**. Only its audio is used, so a voice memo works in its place. Nothing needs to be filmed to make a sample.

---

## Level 0 — dry run, nothing needed from you (~1 session, in the cloud)

**Purpose:** see the *method*. You get the cut rhythm, how lines are matched to clips, the review page and the final file format. It won't show how *your* footage will look.

1. **Script:** Claude writes a ~30s DMS-style script, for example "3 things that fix wobbly lines".
2. **Voice:** Claude generates it phrase by phrase with the built-in synthetic voice. Word timings are known exactly because each phrase is generated separately, so no speech model is needed.
3. **B-roll stand-ins**, one of two options:
   - **0a (free):** Claude generates simple animated "doodle" clips: lines drawing themselves on a paper-coloured background, plus "materials" and "finished piece" style cards, filed into the DMS album folders. They look obviously synthetic, but they exercise motion scoring, contact sheets and matching.
   - **0b (costs Higgsfield credits; only on your OK):** AI-generated 5s vertical clips (hands doodling with a fineliner, a sketchbook flip, a studio desk). These look closer to real B-roll, but they're AI footage, so never post them.
4. **Run the whole skill:** catalog, contact sheets, `shotlist.json`, review page (with safe-zone lines and the wishlist), render, phone copy, `.srt`, draft caption.
5. **Deliverables**, all named `SAMPLE_…`:
   - Review page (HTML)
   - `SAMPLE_dms_broll_edit_phone.mp4`
   - The contact sheets
   - A one-page note: what worked, what to change in the skill

**What you judge:** pace, cut points, whether the review page is useful, and whether the skill's rules need changing.

## Level 1 — your real footage, no new shoot (~15 minutes from you)

**Purpose:** a reel that looks like DMS, built from footage you already have.

From you:

1. **A voice memo,** 30–45s, reading a script Claude writes (or one of your own). Record it on your phone in a quiet room. No camera.
2. **15–25 existing clips** from your camera roll or originals of past Reels/Stories, as varied as possible: hands drawing, materials, finished pieces, studio. Export them from Photos with **File → Export → Export Unmodified Original**; dragging a video out of Photos only gives a still frame.
3. **Answers to the intake questions:**
   - Your handle
   - Do students or children appear in any clip?
   - Anything off limits
   - Pace preference

Then:

- Upload them here, or in the desktop app connect the folder.
- Transcription runs once one of the unblocking options above is in place. Without one, export the memo's auto-captions as `.srt` from CapCut.
- You get the same deliverables as Level 0, plus a **shot wishlist**: the specific clips that would make the next reel better.

**What you judge:** whether this reel, after you add captions and music, is good enough to post.

## Level 2 — the real setup (desktop app, full library)

- Install `dms-broll-editor` in the Claude desktop app. Upload the `.claude/skills/dms-broll-editor/` folder as a skill.
- Install the companion `text-video-editor` skill, which does the transcription. It's on the original Notion page; we only have the main skill file.
- Connect the `DMS-Broll/` and `DMS-Reels/` folders.
- Run a first full catalog (about 5s per clip; started album by album).
- From then on, each reel is: voice memo → review page → render. About 10–20 minutes of your time per reel.

---

## Recommended order

1. **Level 0a now**, free: confirms the method works end to end. If you want stand-in footage that looks more real, approve 0b.
2. **Level 1** once you send a voice memo and a batch of clips.
3. **Level 2** once you're happy with Level 1, so the setup work is only done for something proven.

## Open questions for the studio

- Instagram handle, and the 3–5 kinds of post the page actually makes. The skill currently assumes tips, process, finished pieces, workshops and mission.
- Do children or students appear in footage? Who has consented to appear?
- Where does footage live today: Apple Photos, Google Photos, a drive, the phone only?
- Is any client or commission work private until it's delivered?
