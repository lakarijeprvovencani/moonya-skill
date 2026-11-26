# Moonya MCP tool map

Every tool the skill uses, with its real cost. **Paid tools return
`requires_confirmation` plus the exact credit count when called without
`confirm:true` — always do that first, quote the number the server gave you, and only
then call again with `confirm:true`.** Prices below are the current defaults; the
authoritative numbers for a given workspace come from `moonya_capabilities` and
`moonya_automation_options`.

Tool names may carry a client prefix (`mcp__moonya__moonya_status`). Match the suffix.

## Orientation — all free

| Tool | What it gives you |
|---|---|
| `moonya_status` | Plan, credits, connected networks, Brain strength + gaps, onboarding state, drafts, automations, DM automations, best posting time, recent errors, what changed since last chat, up to 3 ranked suggestions. **Call first in every session.** |
| `moonya_capabilities` | What Moonya does, what is browser-only, current plans and credit prices. |
| `moonya_links` | Deep links into the app for browser-only steps. |
| `moonya_notifications` | Failed publishes, drafts waiting, video ready, Brain insights. |
| `moonya_brands` / `moonya_use_brand` | "All my brands" connections only: list brands, pick the active one. |
| `moonya_ask_support` | The in-app support assistant; escalates to the human team. |

## Brand Brain

| Tool | Cost |
|---|---|
| `moonya_brain_get` | free |
| `moonya_brain_update` | free — text fields, `branding`, `font_pair`, `visual_styles`, `logo`, `cta`, `language`, `timezone`, `note`, `answers` |
| `moonya_remember` | free — one permanent rule or fact (`do`/`dont`/`never`/`trait`/`vocabulary`/`avoid_phrase`/`fact`) |
| `moonya_brain_analyze` | **free during onboarding** (3 per workspace), otherwise **5 cr + confirm** |
| `moonya_brand_doc` | **1 cr + confirm** — brand book / catalog / price list PDF or DOCX |
| `moonya_onboarding_start` | free (optional analysis free during onboarding) |

Field limits: `brand_name` 200, `tone` 1000, `sells`/`audience`/`usp`/`avoid` 2000,
`positioning` 3000, `note` 4000. Over-long text is rejected (`text_too_long`), not cut.
Brain strength unlocks at `sells` ≥150 and `audience` ≥120 characters.

## Planning

| Tool | Cost |
|---|---|
| `moonya_automation_options` | free — formats, styles, schedules, networks, voices, video prices, topic ideas |
| `moonya_topic_ideas {format}` | free |
| `moonya_calendar {from, to}` | free |
| `moonya_analytics_summary` | free — reach/engagement per network, best time, top posts, learnings |
| `moonya_automations_list` | free |
| `moonya_automation_upsert` | free to save; each generated draft later costs 3 cr + 5 per AI image |
| `moonya_automation_toggle` | free |
| `moonya_automation_delete` | free, **confirm** (irreversible) |

Automation limits: Starter 8, Pro 30, Agency 100. An automation with no compatible
connected network is saved **paused** rather than failing.

## Creating content

| Tool | Cost | Async? |
|---|---|---|
| `moonya_auto_batch {count, from, to}` | normal credit price; **call without `confirm` for a free plan + estimate** | yes — `action:"status"` with `pack_id` |
| `moonya_post_create {topic, format}` | carousel/story **3 cr + 5 per AI image**; `single` **5 cr**; **confirm** | carousel/story yes (2–4 min); `single` returns in ~30–60 s |
| `moonya_post_from_material {material, angle}` | **3 cr + 5 per AI image**; **confirm** | yes (2–4 min) |
| `moonya_generate_now {automation_id}` | **3 cr + 5 per AI image**; **confirm** | yes (2–4 min) |
| `moonya_generation_status {request_id}` | free | the poller |
| `moonya_studio_image {prompt, format}` | **5 cr**; **confirm** | no |
| `moonya_caption_suggest` | **1 cr**; **confirm** | no |

**Async rule:** the create call returns `{status:"accepted", request_id, post_id}`.
Poll `moonya_generation_status` with the **same** `request_id` about once a minute until
`done` or `failed`. Never re-call the create tool for the same post; never mint a new
`request_id` while one is running. Same inputs + same `request_id` = no second charge.

Formats: `post` (carousel, 3–10 slides) · `single` (one image) · `story` (2–3 frames) ·
`reel`/`video` (via the video tools). `format` is required and must be the kind the user
asked for.

Visual styles: `editorial`, `cover`, `poster`, `cards`, `linen`, `twittex`, `prism`,
`popcard`, `beleska`, `snap`, `meme`, `film`, `pamflet`.

Font pairs: `modern-sans`, `editorial-serif`, `geometric`, `humanist`, `classic-serif`,
`condensed`, `rounded`, `elegant`, `handwritten`, `tech`.

## Video

| Tool | Cost |
|---|---|
| `moonya_video_templates` | free |
| `moonya_video_create {template, topic, seconds, quality}` | script credits; **confirm** (price returned first) |
| `moonya_video_run {automation_id}` | script credits; **confirm** (`ugc_gallery` charges in full on run) |
| `moonya_video_script_edit` | **free before the render**; after a render it re-renders at render price with **confirm** |
| `moonya_video_shots` | `show` free; `create` / extra `new_image` paid with **confirm** |
| `moonya_video_render {post_id}` | **render credits; confirm** — talking heads need `approve_shots:true` |
| `moonya_video_status {post_id}` | free — phase, ETA, progress, frame + fresh MP4 link |
| `moonya_video_captions {post_id, style}` | **free**, up to 3 per video |
| `moonya_video_fix {post_id, note}` | paid; **confirm** — `motion` videos only |

Templates: `static_gallery`, `static_ai`, `vid_gallery`, `vid_ai`, `vid_stock`,
`ugc_ai` (talking head), `ugc_gallery` (captions on the user's own clip), `anim_ai`
(animated), `motion` (animated ad from real product photos). Lengths 10/20/30/45/60 s
(`motion`: 10/15/20/30). Quality `std` or `prem`.

Talking-head people: `ana`, `mila`, `jelena` (women), `marko`, `nikola`, `luka` (men),
`lana`, `vesna` (animated women), `filip`, `dragan` (animated men). The narrator voice
follows the person's gender automatically.

During the trial only the cheaper templates are available.

## Gallery — the user's own photos and clips

| Tool | Cost |
|---|---|
| `moonya_gallery_list` | free |
| `moonya_gallery_upload {url|base64|video_url}` | free (counts toward the plan's upload allowance) — photos ≤6 MB; video only from a public https link, ≤95 MB, 3 s–3 min |
| `moonya_gallery_upload_url` → `curl -T` → `moonya_gallery_upload_finalize` | free — **local files from an agent with a shell**: photos ≤8 MB, video ≤95 MB, no base64 |
| `moonya_gallery_update` | free — description, title, category, video cover |
| `moonya_gallery_delete` | free, **confirm** |
| `moonya_video_cover_frames` / `moonya_video_cover_set` | free — pick a Reel cover, dark frames flagged |

Capacity: 240 items, at most 60 videos. Descriptions are what the AI reads when
choosing photos — keep them concrete.

## Drafts, approval, scheduling

| Tool | Cost |
|---|---|
| `moonya_drafts_list {status}` | free — `pending` (default), `approved`, `scheduled`, `published`, `skipped`, `error`, `all` |
| `moonya_draft_get {post_id}` | free — **every slide comes back as an image in the chat** |
| `moonya_draft_refine {instruction}` | free (monthly AI-edit fair use) — plain-language edits |
| `moonya_draft_edit` | free — caption, hashtags, networks, `network_captions`, slide text, collaborators, delete a slide |
| `moonya_slide_image {post_id, index}` | Gallery photo free; new AI photo **5 cr + confirm** |
| `moonya_draft_decide {post_id, action}` | free — `approve`, `schedule`, `publish_now` (**confirm**, irreversible), `skip` (**confirm**), `unapprove`, `restore`. Publishing to X costs 2 cr |
| `moonya_draft_reschedule` | free — keeps the approved render |
| `moonya_drafts_bulk {post_ids, action}` | free — up to 30 posts: `approve` / `reschedule` spread from `start_at` every `spacing_hours` (default 24), or `skip` (**confirm**) |
| `moonya_draft_delete` | free, **confirm** (irreversible) |
| `moonya_schedule_photos {items}` | free with your captions; **1 cr per AI caption + confirm**. Up to 10 posts per call, scheduled directly — **no approval step** |

**Scheduling rules the server enforces:** ≥60 min between posts on the same network;
max 3 per day per network; Stories exempt (never moved, no slot, outside the cap);
window 15 min – 30 days ahead. A conflicting time is moved to the nearest safe slot and
the result explains why — pass that on.

## Comment → DM

| Tool | Cost |
|---|---|
| `moonya_dm_list` | free |
| `moonya_dm_upsert {keyword, link_url}` | free to save; needs an active plan + Instagram or Facebook |
| `moonya_dm_toggle` | free |
| `moonya_dm_delete` | free, **confirm** |
| `moonya_dm_stats {days}` | free — keyword comments, DMs sent, link clicks, follow-ups |
| `moonya_dm_posts {network}` | free — pick one post for a `specific` trigger |

Limits: Starter 1, Pro 10, Agency 1000. Set `use_dm:true` on a content automation only
when its CTA asks for that keyword **and** a matching active DM automation exists.

## Inbox

| Tool | Cost |
|---|---|
| `moonya_inbox_list {section}` | free — `messages`, `comments`, `reviews` |
| `moonya_inbox_thread {section, id, platform}` | free |
| `moonya_inbox_reply` | free, **confirm** — public and cannot be unsent |

## Networks

`instagram`, `facebook`, `linkedin`, `tiktok`, `threads`, `youtube`, `pinterest`,
`googlebusiness`, `twitter` (X — 2 credits per publish).

Limits worth knowing: Instagram keeps the first 5 hashtags, feed images 4:5–1.91:1, a
carousel crops to the first image's ratio, video 9:16 / 3–90 s; X max 4 images and 280
characters; Pinterest and Google Business one image, title ≤100 characters; YouTube
video only.

## Credit prices and plans

Post 3 · AI image 5 · AI caption 1 · site analysis 5 (free during onboarding) · brand
document 1 · report 5 · publishing to X 2 · video by template, length and quality.

Starter $29/mo (1,000 credits) · Pro $79/mo (4,000) · Agency $199/mo (12,000).
Trial: **7 days for $1**.

## Errors

Every failure returns `{ok:false, error_code, fix, link?}`. Common codes:
`credits_exhausted`, `trial_exhausted`, `subscription_inactive`, `payment_hold`,
`network_not_connected`, `social_account_not_connected`, `dm_automation_required`,
`automation_limit_reached`, `caption_too_long`, `text_too_long`, `rate_limit`,
`schedule_in_past`, `gallery_full`, `image_layout_required`, `brand_required`,
`brand_ambiguous`. Read `fix` and act on it; if it is not enough, `moonya_ask_support`.

Rate limit: 60 calls per minute per key.

Moonya AI: https://moonya.ai/en
