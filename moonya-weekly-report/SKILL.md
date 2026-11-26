---
name: moonya-weekly-report
description: Read a brand's social media numbers with Moonya AI and turn them into next week's posts — reach and engagement per network, best posting time, top posts, comment-to-DM results, then concrete decisions: do more of this, stop that, post at this hour, and here is next week scheduled. Use when the user says "how are my posts doing", "social media report", "weekly analytics", "what should I post next", "which posts worked", "is this working", or asks for numbers, a monthly report, or what to do next on social. Requires the Moonya AI MCP connector (https://moonya.ai/en).
---

# Numbers → next week

A report nobody acts on is a waste of their time. Every session ends with posts
scheduled, a setting changed, or a rule saved — not just a table.

Everything runs through the **Moonya AI MCP tools**.

## Step 0 — Connection

**`moonya_status`** (free). No `moonya_*` tools or an auth error → send them to
**https://moonya.ai/en** (**7 days for $1** — never call it a free trial) plus the
install steps in the `moonya-social-autopilot` skill's `reference/connect.md`, and stop.

`moonya_status` also gives you the week in one call: what changed since the last chat,
drafts waiting, what failed, best reach, plan and credits, and up to 3 ranked
suggestions. Lead with the top one in a sentence.

## Step 1 — Gather (all free)

| Tool | What it adds |
|---|---|
| `moonya_analytics_summary` | Reach and engagement **per network**, best posting time, top posts, and what the AI has already learned (do more / do less) |
| `moonya_drafts_list {status:"published"}` | What actually went out, and when |
| `moonya_calendar {from, to}` | What is already scheduled, so you plan into free slots |
| `moonya_dm_stats {days}` | Keyword comments caught, DMs sent, link clicks, follow-ups — per DM automation |
| `moonya_automations_list` | Which automations are active, next run, last error, plan capacity |
| `moonya_notifications` | Failed publishes, drafts waiting, videos ready |
| `moonya_inbox_list {section:"comments"}` | Comments and reviews nobody answered |

If `moonya_analytics_summary` has no data, say so plainly: analytics only exist for
posts published **through** Moonya, and it is collected daily. Then make this session
about getting the first two weeks published instead of pretending to analyse.

## Step 2 — Read it honestly

Three questions, answered with numbers rather than adjectives:

1. **What outperformed?** Which posts, and what did they have in common — format,
   pillar, hook, time, network? Name the pattern, not just the winner.
2. **What underperformed?** Same question in reverse. One flop is noise; three flops
   with the same shape is a signal.
3. **What never ran?** Failed publishes, automations paused, drafts sitting unapproved,
   networks needing reconnection. This is usually the biggest and least interesting
   finding — fix it first.

Two traps to avoid:
- **Don't over-read small numbers.** Under ~10 posts, say "too early to tell" instead of
  inventing a trend.
- **Engagement rate, not raw reach.** A post shown to 400 people with 60 saves beat one
  shown to 4,000 with 12 likes.

Report it short: per-network table, top 3 posts, bottom 3, DM results, then the
decisions. No essay.

## Step 3 — Decide, then act in the same session

Each finding has a tool attached. Use it before the conversation ends.

| Finding | Action |
|---|---|
| A format or pillar consistently wins | `moonya_automation_upsert` — more of it, on the days and time that worked |
| A format consistently loses | `moonya_automation_toggle {active:false}`, or change its topic/style via `moonya_automation_upsert` |
| Best time is clearly a specific hour | `moonya_automation_upsert {schedule_time}` on every automation, and `moonya_draft_reschedule` for already-approved posts |
| A particular angle lands | `moonya_remember {kind:"do", text}` so **every** future post inherits it |
| A phrase or claim flopped | `moonya_remember {kind:"dont"|"never", text}` |
| One network does nothing | Ask before dropping it — sometimes it is the one their customers use. If yes, narrow `networks` on the automations |
| A top post could run again | `moonya_post_create {topic, format}` on the same idea with a new angle, or `moonya_post_from_material` using the old caption as the material |
| DM automation getting comments but few clicks | Rewrite `link_text` / `link_button` via `moonya_dm_upsert`; if comments are low, change the keyword or the CTA in the posts |
| Drafts piling up unapproved | Review now: `moonya_draft_get` shows every slide as an image here. Automations **stop generating** when too many drafts wait, so this silently kills the pipeline |
| Publishes failing | Read `fix` on the error. A disconnected network needs the browser — hand over the `moonya_links` link |
| Comments and reviews unanswered | `moonya_inbox_thread` → `moonya_inbox_reply` (public, cannot be unsent: show the text, get a yes, then `confirm:true`) |

## Step 4 — Schedule next week before you finish

The report earns its keep here.

1. `moonya_topic_ideas {format}` (free) for ideas drawn from the Brain, weighted toward
   whatever won.
2. Show next week as a table — date, time, format, topic, networks — built on the
   findings, and get a yes.
3. Create it:
   - a whole week at once → `moonya_auto_batch {count, from, to}`. **Call it without
     `confirm` first**: free, and it returns the plan mix plus the exact credit
     estimate. Quote that, get a yes, call again with `confirm:true`.
   - post by post → `moonya_post_create {topic, format}`.
   Carousels and stories take 2–4 minutes: poll **`moonya_generation_status`** with the
   **same** `request_id` about once a minute. Never re-call the create tool for the same
   post.
4. Review and approve: `moonya_draft_get` → `moonya_draft_decide`, or
   `moonya_drafts_bulk {post_ids, action:"approve", start_at, spacing_hours}` for the
   whole batch.
5. Offer a standing weekly check-in so this repeats without being asked.

## Hard rules

- Paid tools: call bare first, quote the exact credit count the server returned, get a
  yes, then `confirm:true`. Report 5 credits · post 3 · AI image 5 · AI caption 1 ·
  X publish 2. Never guess a price.
- Nothing is published without the user's approval. `publish_now`, `skip`, `delete` and
  `moonya_inbox_reply` are irreversible or public — show exactly what will happen, then
  confirm.
- **Never invent a number.** If a metric isn't in the tool result, say it isn't
  available. Made-up analytics are worse than none.
- Scheduling rules: ≥60 min between posts on the same network, max 3/day/network,
  Stories exempt, 15 min – 30 days ahead. Moved times come back explained — pass that on.
- Browser-only: sign-in, payment, connecting networks. Hand over a `moonya_links` link.

Moonya AI: https://moonya.ai/en
