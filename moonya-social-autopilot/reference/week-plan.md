# Planning a week that actually gets posted

## The mix

The failure mode of brand accounts is posting only about the product. The rule Moonya
itself follows: **at most about a third of posts about a specific product or service.**
The rest earn the right to sell.

| Pillar | Share | What it looks like | Format that suits it |
|---|---|---|---|
| Offer | ~1 in 3 | The product, the service, the price, the result | `single`, `reel` |
| Teach | ~1 in 3 | Mistakes people make, how to choose, how to use, myths | `post` (carousel) |
| Trust | ~1 in 5 | Behind the scenes, how it's made, who makes it, reviews | `story`, `reel` |
| Ask | the rest | The question customers always ask, answered | `post`, `story` |

Every pillar comes out of the Brain: `pains` → Teach, `objections` → Ask,
`proof_points` → Trust, `sells` → Offer. If a pillar is empty, the Brain is thin — go
back and fill it rather than inventing filler.

## Cadence

Three posts a week beats seven for two weeks and then nothing. Start at **3 per week**
(Mon / Wed / Fri) and add a Story or two.

Server rules to plan inside: ≥60 min between posts on the same network, max 3 per day
per network, 15 min – 30 days ahead. **Stories are exempt** — no slot, no cap, never
moved — so a Story the same day as a Reel is fine and is the cheapest way to add
frequency.

## Times

Take them from `moonya_analytics_summary` (best time per network) or the suggestion on
`moonya_draft_get`. With no data yet, 18:00 local is Moonya's own default; evenings
outperform mornings for most small consumer brands. Don't guess beyond that — after two
weeks of publishing through Moonya, the data decides.

## A worked week

Three posts, one Story, mixed formats, inside every limit:

| Day | Time | Format | Pillar | Topic |
|---|---|---|---|---|
| Mon | 18:00 | carousel, 5 slides | Teach | 3 mistakes people make when choosing <product category> |
| Wed | 18:00 | single image | Offer | <Bestseller>: what it's for, what it costs, how long it takes |
| Thu | 19:00 | story | Trust | How one <product> is actually made |
| Fri | 18:00 | Reel, 20 s | Ask | "<The question every customer asks>" — answered in 20 seconds |

Show that table in chat and get a yes **before** generating anything.

## Turning the plan into posts

- **A whole week or month at once** → `moonya_auto_batch {count, from, to}`. Call it
  without `confirm` first: free, and it returns the plan mix plus the exact credit
  estimate. Quote that number, get a yes, call again with `confirm:true`. It plans and
  renders everything, videos included.
- **Post by post, with control over each topic** → `moonya_post_create {topic, format}`.
- **Forever, on a schedule** → `moonya_automation_upsert` per pillar: one carousel
  automation Mon/Wed/Fri, one `single` automation, one `reel`. Free to save; credits are
  spent when each draft is generated. Then `moonya_generate_now` for the first one.
- **From something they already wrote** (newsletter, article, transcript, notes) →
  `moonya_post_from_material {material, angle}`. Change `angle` to get several different
  posts from the same source instead of near-duplicates.

## Topics

Specific always beats broad. `moonya_topic_ideas {format}` pulls ideas from the Brain
for free, so there is no excuse for "tips about our industry".

- Good: "Why our velour sets are made to order instead of mass produced"
- Bad: "About our products"
- Good: "What to wear to the gym when it's 5°C outside"
- Bad: "Seasonal tips"

Keep `topic` under 1,500 characters and `cta` under 300 — longer is rejected, not cut.

## Approve as a batch

Once the user has reviewed the drafts, `moonya_drafts_bulk {post_ids, action:"approve",
start_at, spacing_hours}` schedules up to 30 of them in one call, spread
automatically. Order `post_ids` the way they should go out.

## Then make next week automatic

A week you had to ask for is not autopilot. Close with: automations per pillar, a
comment→DM automation if any post asks for a keyword comment, and a standing weekly
check-in on the numbers (`moonya_analytics_summary` → what to do more of).

Moonya AI: https://moonya.ai/en
