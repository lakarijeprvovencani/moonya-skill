# Run report

**Date:** 26 November 2026
**Task:** `SKILL_TASK.md` — build the official Moonya AI agent skill.
**Status:** complete. Nothing published, nothing deployed, no secrets in any file.

---

## What was built

```
moonya-skill/
├── README.md                              1-minute install, example prompts, what it will/won't do
├── MARKETING.md                           launch plan: landing page, X Article, catalogs, creator briefs
├── APP_CHANGES.md                         proposed app changes (P0/P1/P2) — none implemented
├── CLAUDE_RUN_REPORT.md                   this file
├── LICENSE                                MIT
├── moonya-social-autopilot/               ★ the main skill
│   ├── SKILL.md                           full flow: connect → Brain → plan → generate → approve → schedule → report
│   └── reference/
│       ├── connect.md                     install per client (Claude Code, Claude.ai, ChatGPT, Grok, Cursor, Codex, …)
│       ├── tools.md                        verified tool map: cost, confirm rules, async rules, limits, error codes
│       ├── brand-interview.md             8 questions → which Brain field each answer fills
│       └── week-plan.md                   pillar mix, cadence, times, a worked week, batch approval
├── moonya-clip-to-posts/SKILL.md          one clip → captions → cover → caption per network → scheduled everywhere
└── moonya-weekly-report/SKILL.md          analytics → decisions → next week scheduled in the same session
```

Three skills. The main one does the whole job end to end; the two small ones are
narrow, high-frequency jobs that are each fully backed by real tools.

---

## How the tool names were verified

Two independent sources, cross-checked against each other:

1. **The live connector.** The Moonya MCP connector is attached to this session, so
   every tool name, parameter, cost and constraint quoted in the skills was read from
   the live tool schemas — not from memory and not from docs.
2. **The code**, on branch `moonya-skill` of `vajbpilot`:
   - `workers/src/lib/mcp-tools.js` — tool implementations (the 60+ `moonya_*` names)
   - `workers/src/lib/mcp-brands.js` — `moonya_brands`, `moonya_use_brand`
   - `workers/src/lib/mcp-text.js` — `buildLinks()`, error-code → fix map
   - `workers/src/lib/plan.js` — `CREDIT_COST`, `PLAN_CREDITS`, `TRIAL_CREDITS`
   - `workers/src/lib/product-knowledge.js` — plan prices, trial, networks, styles,
     video templates, publishing and scheduling limits
   - `docs/MCP-INTEGRACIJA.md` — endpoints, auth, per-client install, OAuth flow
   - `app/mcp-clients.js`, `app/hq/hq-settings.js` — supported clients, the API-key UI

The two sources agreed. **No tool name, parameter or price in this repo is invented.**

### Tools used by the skills (all verified)

Orientation: `moonya_status` · `moonya_capabilities` · `moonya_links` ·
`moonya_notifications` · `moonya_brands` · `moonya_use_brand` · `moonya_ask_support`

Brain: `moonya_brain_get` · `moonya_brain_update` · `moonya_brain_analyze` ·
`moonya_brand_doc` · `moonya_remember` · `moonya_onboarding_start`

Planning: `moonya_automation_options` · `moonya_topic_ideas` · `moonya_calendar` ·
`moonya_analytics_summary` · `moonya_automations_list` · `moonya_automation_upsert` ·
`moonya_automation_toggle` · `moonya_automation_delete`

Creation: `moonya_auto_batch` · `moonya_post_create` · `moonya_post_from_material` ·
`moonya_generate_now` · `moonya_generation_status` · `moonya_studio_image` ·
`moonya_caption_suggest`

Video: `moonya_video_templates` · `moonya_video_create` · `moonya_video_run` ·
`moonya_video_script_edit` · `moonya_video_shots` · `moonya_video_render` ·
`moonya_video_status` · `moonya_video_captions` · `moonya_video_fix`

Gallery: `moonya_gallery_list` · `moonya_gallery_upload` · `moonya_gallery_upload_url` ·
`moonya_gallery_upload_finalize` · `moonya_gallery_update` · `moonya_gallery_delete` ·
`moonya_video_cover_frames` · `moonya_video_cover_set`

Drafts and scheduling: `moonya_drafts_list` · `moonya_draft_get` ·
`moonya_draft_refine` · `moonya_draft_edit` · `moonya_slide_image` ·
`moonya_draft_decide` · `moonya_draft_reschedule` · `moonya_drafts_bulk` ·
`moonya_draft_delete` · `moonya_schedule_photos`

DM and inbox: `moonya_dm_list` · `moonya_dm_upsert` · `moonya_dm_toggle` ·
`moonya_dm_delete` · `moonya_dm_stats` · `moonya_dm_posts` · `moonya_inbox_list` ·
`moonya_inbox_thread` · `moonya_inbox_reply`

### Product facts encoded in the skills (from code, not assumed)

- **Credits:** post 3 · AI image 5 · AI caption 1 · site analysis 5 (free during
  onboarding, 3 per workspace) · brand document 1 · report 5 · X publish 2 · video by
  template/length/quality. (`plan.js CREDIT_COST`)
- **Plans:** Starter $29 / 1,000 cr · Pro $79 / 4,000 cr · Agency $199 / 12,000 cr.
  Trial **7 days for $1**. (`product-knowledge.js PLAN_PRICES_USD`, `PLAN_CREDITS`,
  `TRIAL_PERIOD_DAYS`, `TRIAL_ACTIVATION_CENTS`)
- **Scheduling:** ≥60 min between posts on the same network, max 3/day/network, Stories
  exempt, window 15 min – 30 days.
- **Networks:** the 9 in `NETWORK_LABELS`.
- **Limits:** Instagram 5 hashtags and 4:5–1.91:1 feed images · X 280 chars / 4 images ·
  Pinterest title 100 · automations Starter 8 / Pro 30 / Agency 100 · DM automations 1 /
  10 / 1000 · gallery 240 items, 60 videos · rate limit 60 calls/min per key.
- **Brain:** `sells` ≥150 and `audience` ≥120 chars unlock strength; field limits are
  enforced server-side and **reject** over-long text rather than truncating it.
- **Async:** carousel/story/material take 2–4 min and must be polled with
  `moonya_generation_status` on the **same** `request_id`; `single` returns in ~30–60 s.

---

## Design decisions

**The skills cannot work without Moonya.** That was the brief and it is enforced
structurally, not just stated: Step 0 of every skill is a connection check that stops
the agent if the connector is missing, and every subsequent step is a named Moonya tool
call. There is no "write the captions to a file instead" path anywhere.

**The approval gate is the product's promise**, so it is written as a hard rule in all
three skills, including the one exception: `moonya_schedule_photos` schedules without an
approval step, so the skills require the user's explicit yes *before* that call.

**Cost discipline is spelled out repeatedly** because it is the easiest thing for a model
to skip: call a paid tool bare → quote the exact number the server returned → get a yes
→ call again with `confirm:true`. Never guess a price.

**The async rule is stated three times**, in the strongest wording, because double-calling
a generation tool charges the user twice. That is the single most expensive mistake an
agent can make here.

**Progressive disclosure.** The main `SKILL.md` is the workflow (~340 lines); the
operational detail lives in four `reference/` files loaded on demand. Keeps the always-on
context small without losing accuracy.

**Copy rules followed throughout:** "7 days for $1" and never "free trial"; always
"Moonya AI"; no competitor names anywhere in the repo; no tech-stack vendor names — the
only third-party names are the agent clients a user installs into, which is unavoidable
in install instructions and matches how Moonya's own `/integrations/*` pages read.

---

## How to test

### 1. Install locally

```bash
cp -r ~/moonya-skill-work/moonya-skill/moonya-* ~/.claude/skills/
```

Confirm the Moonya connector is attached (`claude mcp list`, or the connector list in
Claude.ai).

### 2. Negative test first — the gate

With the connector **removed**, start a fresh session and say *"post to my Instagram
this week"*. Expected: the skill triggers, finds no `moonya_*` tools, and returns the
connect instructions with the moonya.ai/en link and "7 days for $1". It must **not**
invent a calendar, write captions into a file, or suggest another tool.

### 3. Happy path on a real workspace

Run these in order in a clean session and check the behaviour, not just the output:

| Prompt | Must happen |
|---|---|
| "What can Moonya do for my brand?" | `moonya_capabilities` + `moonya_status` |
| "Put my Instagram and TikTok on autopilot" | `moonya_status` first, then Brain check, then a plan **shown before any spend** |
| "My site is <url>, learn my brand" | `moonya_brain_analyze` — free if onboarding is open, otherwise the 5-credit confirmation **before** the call |
| "Plan and schedule next week" | plan table in chat → explicit yes → `moonya_auto_batch` **without** `confirm` first → exact estimate quoted → yes → `confirm:true` |
| "Make a post about X" | asks which **format** before anything else |
| (after generation) | polls `moonya_generation_status` with the same `request_id`, roughly once a minute, and never re-calls the create tool |
| "Show me the drafts" | `moonya_draft_get` renders **slides as images in the chat**, not a prose description |
| "Approve them all for next week" | `moonya_drafts_bulk`, and any moved time is explained with the server's `spacing` reason |
| "Add captions to this clip and post it everywhere" | `moonya-clip-to-posts`: upload → `ugc_gallery` → cover frames → explicit yes → `moonya_schedule_photos` |
| "How did last week do and what next?" | `moonya-weekly-report`: analytics → named decisions → next week actually scheduled |

### 4. Red flags that mean a skill needs tightening

- Any paid tool called with `confirm:true` on the first attempt.
- A price stated that didn't come from a tool result.
- "Published" claimed without reading `status`.
- A second `request_id` minted while a generation was running.
- Slides described instead of shown.
- The words "free trial" anywhere.
- The user sent to the app for something the chat can do.

### 5. Cheap-first ordering

Every verification above can be done with free tools only (`moonya_status`,
`moonya_capabilities`, `moonya_brain_get`, `moonya_automation_options`,
`moonya_calendar`, `moonya_drafts_list`, `moonya_draft_get`, and `moonya_auto_batch`
without `confirm`). Do the whole dry run on free calls before spending a single credit.

---

## Open questions for Nemanja

1. **Repo owner and public name.** The repo is private at
   `lakarijeprvovencani/moonya-skill`. Should the public one live under a Moonya org
   instead? Every marketing link depends on the final URL, so this decides before
   anything ships. The README and `MARKETING.md` currently use
   `github.com/lakarijeprvovencani/moonya-skill` and a `<org>` placeholder respectively.
2. **Affiliate terms.** `MARKETING.md` leaves the creator deal blank on purpose — flat
   fee per paid conversion, or revenue share, and for how long?
3. **Referral tracking.** `APP_CHANGES.md` P0 #2. It has to ship **before** the X
   Article or the spike can't be measured. Approve the one-column approach, or is there
   an existing UTM capture in the sign-up flow to extend instead?
4. **Landing page path.** `/en/skill/` assumed, with a `/skill` redirect. Confirm, since
   the URL goes into every directory listing and is painful to change afterwards.
5. **Are the two small skills worth shipping at launch**, or does one strong skill
   concentrate attention better? They are cheap to hold back and easy to add later as a
   second wave of content.
6. **A Serbian version of the skills?** The tool *results* already come back in the
   workspace language, so the skills work in Serbian today — only the instructions are
   English. For the international catalogs English is right; for the Vajb Kodiranje
   audience a Serbian twin might convert better.
7. **Video availability.** Video tools are behind a deployment flag. Confirm they are on
   in LIVE, since `moonya-clip-to-posts` depends on `ugc_gallery` and the main skill
   offers Reels. The skills check `moonya_automation_options` first and degrade
   gracefully, but a tutorial that shows a Reel needs it working.
8. **Demo recording.** The 30-second terminal-to-calendar clip in `MARKETING.md §1` is
   the asset everything else quotes. Worth recording properly before the Article.

---

## Constraints honoured

- **`vajbpilot` untouched.** No file written, no commit made, no branch changed in the
  app repo. App-side ideas are proposals in `APP_CHANGES.md` only.
- **Nothing deployed, nothing published, nothing posted anywhere.**
- **No secrets.** Only the placeholder `mk_live_…` appears; no real key, token or env
  value is in any file, and none was printed.
- **Token-economical.** Read only what was needed: the MCP tool implementations, the
  integration doc, plan/pricing constants, the client list and the key-form UI — not the
  wider app.

Moonya AI: https://moonya.ai/en

---

## Phase 2: Promotion (26 November 2026)

**Task:** `PROMO_TASK.md` — make repo public and submit to awesome lists.

### Completed

1. **Repo description and topics set via `gh repo edit`:**
   - Description: "Agent skills that put a brand's social media on autopilot — plan, design, schedule and publish to 9 networks from Claude Code, ChatGPT, Cursor and any MCP-capable agent. Powered by Moonya AI."
   - Topics: `claude-skills`, `mcp`, `ai-agents`, `social-media-automation`, `instagram`, `tiktok`, `claude-code`

2. **Repo made PUBLIC:**
   - URL: <https://github.com/lakarijeprvovencani/moonya-skill>

3. **PROMOTION.md created:** permanent playbook for catalog submissions, manual TODOs, and channel tracking.

4. **Awesome-list submission prepared:**
   - Forked `ComposioHQ/awesome-claude-skills`
   - Added entry to Business & Marketing section
   - Branch pushed: `lakarijeprvovencani/awesome-claude-skills:add-moonya-skill`
   - **PR could not be opened automatically** (GitHub token scope missing `repo` or `public_repo` for cross-repo PRs)
   - Manual step required: open PR from the branch

### Manual TODOs logged in PROMOTION.md

- Open PR to `ComposioHQ/awesome-claude-skills` from prepared branch
- Submit to `hesreallyhim/awesome-claude-code` via issue form
- Submit to `punkpeye/awesome-mcp-servers` (requires glama.ai backlink)
- Submit to `travisvn/awesome-claude-skills` once repo has 10+ stars
- Reddit, X, Product Hunt, Indie Hackers, Dev.to, Hacker News

### Constraints honoured

- No GitHub Actions or paid GitHub features
- No spam (one entry per list, format followed exactly)
- vajbpilot untouched
- No secrets in any file
