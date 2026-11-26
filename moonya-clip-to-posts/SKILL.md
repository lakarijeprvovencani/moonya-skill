---
name: moonya-clip-to-posts
description: Turn one video clip into posts for every social network with Moonya AI — upload the clip, add branded captions burned into it, pick a cover frame that isn't black, write a caption per network, and schedule it as a Reel, a TikTok, a Short, a Story and a feed post. Use when the user says "repurpose this video", "turn this clip into posts", "post this video everywhere", "make this a Reel and a TikTok", "add captions to my video", "I filmed something, do something with it", or hands over a video file or link and wants it on social. Requires the Moonya AI MCP connector (https://moonya.ai/en).
---

# One clip → every network

The user filmed one thing. Your job is to get it onto every network it fits, with
captions, a decent cover and a caption written for each platform — without them opening
an editor.

All of this runs through the **Moonya AI MCP tools**. No connector, no job: run the
connection check first.

## Step 0 — Connection and clip

1. **`moonya_status`** (free). No `moonya_*` tools or an auth error → send them to
   **https://moonya.ai/en** (**7 days for $1** — never call it a free trial) and the
   install steps in the `moonya-social-autopilot` skill's `reference/connect.md`. Stop
   there.
2. Find the clip:
   - **Already in the Gallery** → `moonya_gallery_list`, look for items with kind
     `raw_clip`.
   - **A local file and you have a shell** → `moonya_gallery_upload_url {filename,
     content_type, size_bytes, kind:"video"}` → run the returned `curl -T` PUT →
     `moonya_gallery_upload_finalize {upload_id}`. Get the exact size first
     (`stat -f%z file` on macOS, `stat -c%s file` on Linux); the upload must match it.
   - **A public https link** → `moonya_gallery_upload {video_url}`.
   - **A chat client with no shell and a local file** → they upload it in the Gallery in
     the app; give them the link from `moonya_links`. Be straight about it.

   Limits: MP4 or MOV, H.264, **≤95 MB, 3 s – 3 min**. Gallery holds 240 items, max 60
   videos.
3. Give the clip a real description with `moonya_gallery_update {description}` — it is
   what the AI reads when writing captions and choosing shots.

## Step 1 — Watch the clip with them

Ask two questions, not ten: **what happens in it**, and **what the viewer should do
after watching**. That is everything the caption and CTA need.

Then check what it can go on. Vertical 9:16 is the best case — Reel, TikTok, Short,
Story, all of it. Instagram video must be 9:16 and 3–90 s; YouTube is video only; a feed
*image* post needs 4:5–1.91:1, so a vertical clip goes as a Reel or Story, not a feed
image.

## Step 2 — Burned-in captions (the version that gets watched)

Most of these are watched on mute, so captions are not optional.

`moonya_video_create {template:"ugc_gallery", topic, gallery_media_ids:[clip_media_id],
seconds}` — the "captions on your own clip" template: Moonya transcribes what is said
and burns branded captions over the user's footage.

- Called **without** `confirm` it returns the exact price. Quote that, get a yes, then
  call again with `confirm:true`. (`ugc_gallery` is charged in full on run, not in two
  stages like other templates.)
- Follow it with **`moonya_video_status {post_id}`** until `ready` — then show the frame
  and the MP4 link right here.
- **`moonya_video_captions {post_id, style}`** is **free** and re-assembles the same
  video with a different caption look: `brand`, `white`, `yellow`, `box`, `elegant`. Up
  to 3 changes. Offer it — it costs nothing and it is the single most visible
  improvement. Don't schedule the post while a new version is rendering.

If the clip has nothing spoken in it, skip this step and schedule the raw clip in
Step 4.

## Step 3 — A cover that isn't black

Instagram grabs the first frame unless you tell it otherwise, and first frames are
usually mid-blink or dark.

1. **`moonya_video_cover_frames {media_id}`** — free. Returns up to 6 frames as images
   with brightness and a `dark` flag, plus a `recommended_index`.
2. Show them, recommend the best non-dark one in one sentence, **ask which they want**.
3. **`moonya_video_cover_set {media_id, candidate_id}`** — free.

To use an existing Gallery photo as the cover instead: `moonya_gallery_update
{media_id, cover_media_id}`.

## Step 4 — Captions per network, then schedule

1. **Caption.** Either write it yourself from what they told you in Step 1, or
   `moonya_caption_suggest {media_id|hint}` (1 credit, `confirm:true`) for one in the
   brand voice. Instagram keeps only the first **5 hashtags**, so put the 3–5 that
   matter first.
2. **Schedule.** `moonya_schedule_photos {items:[{video_media_id, scheduled_at,
   caption, networks}]}`:
   - the clip as a **Reel / video post** → `video_media_id`
   - the same clip as a **Story** → a second item with `story:true` (max 60 s)
   - `cover_media_id` per item if you want to override the saved cover
   - up to **3 Instagram `collaborators`** on a feed post, carousel or Reel — they have
     to accept the invite in Instagram for it to appear on their profile
   - **These go straight to the calendar as scheduled — there is no approval step.** So
     show the user the exact list, times and networks and get a clear yes *before* the
     call.
   - Captions you supply are free; a missing caption is written by the AI for 1 credit
     each and needs `confirm:true`.
3. **Tune per network afterwards** with `moonya_draft_edit {post_id, network_captions}`
   — X maxes out at 280 characters and prefers 1–2 hashtags, LinkedIn wants a
   professional tone and ≤3 hashtags, Threads and Pinterest cap at 500, YouTube and
   Pinterest titles at 100.

Server scheduling rules: ≥60 min between posts on the same network, max 3 per day per
network, 15 min – 30 days ahead. **Stories are exempt** — so the Story version can go
out the same day as the Reel without taking a slot. A conflicting time is moved to the
nearest safe slot and the result tells you why: pass that on instead of reporting a
time the user didn't choose.

## Step 5 — Squeeze the rest out of it

One shoot should not be one post. Offer, in this order:

- **A carousel from the same idea** — `moonya_post_create {topic, format:"post"}`, where
  the topic is what the clip says. Same message, different surface, different audience.
- **A still from the footage as a feed post** — pick a frame with
  `moonya_video_cover_frames`, then schedule it as a photo post.
- **Comment → DM**, if the clip asks people to comment something:
  `moonya_dm_upsert {keyword, link_url}` — free to save, needs Instagram or Facebook
  connected, and turns comments into DMs with their link automatically.
- **The same clip again in 3–4 weeks** with a different caption and hook. The cheapest
  content there is.

## Hard rules

- Every paid tool returns `requires_confirmation` plus the exact credit count when
  called without `confirm:true`. Always do that first, quote the server's number, get a
  yes, then confirm. Never guess a price.
- `moonya_schedule_photos` schedules without an approval gate — the user's yes replaces
  it. Get it explicitly.
- Report the status the tool returned. Never claim something is live because you called
  a tool.
- Browser-only: sign-in, payment, connecting networks, uploading a video from a chat
  client with no shell, TikTok privacy settings. Hand over a `moonya_links` link and say
  so plainly.
- Errors come back as `{ok:false, error_code, fix}` — read `fix` and act on it;
  `moonya_ask_support` if it isn't enough.

Moonya AI: https://moonya.ai/en
