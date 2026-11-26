# Launch plan — the Moonya skills

**Internal document. Not published.** Public copy is marked with `> PUBLIC COPY` and
follows the rules at the bottom.

## The bet

A free, genuinely useful agent skill that **cannot work without Moonya** is a better
acquisition channel than daily content, because:

1. It is distributed by other people. A tutorial written by someone else, in their
   voice, for their audience, converts better than our own ad — and costs nothing.
2. It lands in a buying moment. Someone installing a social-media autopilot skill in
   their agent already wants a social-media autopilot.
3. It is permanent. A skill in a repo, a catalog listing and an awesome-list entry keep
   sending signups for months. A post does not.
4. The entry price removes the decision. **7 days for $1** is not a trial someone has to
   think about.

The skill is useless without the connector, so every install is a qualified trial
candidate. That is the whole mechanism — protect it. Never ship a "works standalone"
version.

## Sequence

| # | Move | Why it's in this order |
|---|---|---|
| 1 | Landing page at `moonya.ai/en/skill` | Everything else links here. Ship before any outreach. |
| 2 | Public GitHub repo + README | The thing people actually install. Must be live before a single link goes out. |
| 3 | Catalog and awesome-list submissions | Slow to approve (days to weeks), so start them early and in parallel. |
| 4 | The X Article | The one asymmetric bet. Needs 1 and 2 live and working. |
| 5 | Dev-community how-tos (dev.to, Reddit, Hashnode) | Reuse the Article's structure, rewritten per audience. |
| 6 | Micro-creator tutorial briefs | Scale what worked in 4 and 5 through other people's audiences. |
| 7 | In-app and onboarding surfacing | Existing users become the skill's second distribution channel. |

Don't launch 4 before 1 and 2 are live and tested end to end on a clean machine. One
broken install instruction wastes the only spike you get.

---

## 1. Landing page — `moonya.ai/en/skill`

Build spec is in `APP_CHANGES.md`. Outline:

1. **Hero.** `Put your Instagram and TikTok on autopilot — from your AI agent.`
   Sub: *Install one free skill. Tell your agent what your business does. It plans,
   designs, schedules and publishes your posts across 9 networks — you approve every
   one.* Primary CTA **Start — 7 days for $1**, secondary **Get the skill (free)** → repo.
2. **The 30-second demo.** A terminal/chat recording: one sentence in → a week of posts
   out → the approval screen with real slides. No narration needed. This is the asset
   that gets quoted everywhere else; make it good.
3. **Three steps.** Sign up (7 days for $1) → connect the connector (one command, shown
   for the top 6 clients with copy buttons) → install the skill. Each step one line.
4. **What it does, in the user's words.** Six example prompts as copyable chips, taken
   straight from the README.
5. **The three skills**, one card each, with the one-line description.
6. **What needs a browser.** Short and honest. Builds trust and cuts support tickets.
7. **Approval is the headline feature.** Screenshot of slides in the chat + the approve
   step. "Nothing is published without you."
8. **Networks strip.** All nine logos.
9. **Pricing.** 7 days for $1 → Starter $29 / Pro $79 / Agency $199. Credits explained
   in one sentence.
10. **FAQ.** Which agents? Do I need to code? Can it post without asking me? What if I
    have no website? Can I use it for clients' accounts? What happens after the 7 days?
11. **Final CTA.** Same as hero.

SEO: title `Social media autopilot skill for AI agents | Moonya AI`. Target
*"social media MCP"*, *"social media agent skill"*, *"schedule Instagram from Claude
Code"*, *"post to Instagram with an AI agent"*, *"MCP social media autopilot"*.
Add to `sitemap.xml` and `llms.txt`, and cross-link from `/docs/mcp/` and every
`/integrations/*` page.

---

## 2. The X Article

The single highest-variance move: an Article is long-form, indexed, quotable and
re-shareable, and it rewards a concrete build over an opinion.

**Title options**
- `How I put my Instagram on autopilot with Claude Code (and never opened a scheduler again)`
- `I gave my AI agent my Instagram password. Here's what it did instead.`
- `My AI agent now runs my shop's social media. Here's the exact setup.`

**Structure — hook first, build second, copy-this last**

1. **Hook (first 3 lines — this is the whole game).**
   *"I haven't opened a social scheduler in six weeks. I type one sentence into my
   terminal and next week's posts are designed, written and waiting for my approval. My
   shop's Instagram, TikTok and Facebook have not had a gap since. Here's the exact
   setup — it took me eleven minutes."*
   Concrete, first-person, no product name yet.
2. **The problem, honestly.** Scheduling tools schedule; they don't create. AI writers
   write; they don't know your business or publish. The gap between "I know I should
   post" and a post actually going out is where small brands die.
3. **What I actually built.** The skill + the connector, one paragraph, no architecture
   diagram.
4. **The real transcript.** The single most important section. One unedited session:
   the prompt, the brand questions, the plan table, the generated slides as images, the
   approval, the scheduled calendar. Screenshots, not description. People believe
   transcripts.
5. **The part that surprised me.** Something specific and non-obvious — e.g. it refused
   to publish until I approved, told me the exact credit cost before every spend, and
   moved a post because two were too close together on the same network. Specificity is
   what makes it read as real rather than promotional.
6. **What it costs.** Skill free, Moonya 7 days for $1 then $29/mo. State it plainly —
   hiding it costs more trust than it saves.
7. **"This is how you can copy this exact model."** The actual install, copy-pasteable:
   sign up link, connector command, skill clone, first prompt. Four blocks. Then the
   generalisation: *any* business process you do weekly can be wrapped as a skill over
   an MCP — this one just happens to be social media.
8. **One CTA.** Repo link + moonya.ai/en. Not three.

**Rules**
- One CTA. Every extra link halves the first.
- Screenshots over claims. Transcript over summary.
- No competitor names, no "best tool" framing, no invented metrics. If we don't have a
  number yet, we don't publish one.
- Reply to every comment in the first 24 hours; replies are what keep an Article's
  distribution alive.

**Reuse the same body for**: dev.to, Hashnode, a LinkedIn article, and a Reddit post
(rewritten — Reddit punishes copy-paste; lead with the transcript, keep the link to a
comment).

---

## 3. Where to list it

Free, and these are the places both humans and LLMs search when someone wants an MCP
or a skill. One owner, one tracking sheet, check back weekly — most need a nudge.

**MCP server directories**
- `github.com/modelcontextprotocol/servers` — community servers list (PR)
- `github.com/punkpeye/awesome-mcp-servers` (PR)
- `github.com/wong2/awesome-mcp-servers` (PR)
- `mcpservers.org`
- `mcp.so`
- `smithery.ai`
- `glama.ai/mcp/servers`
- `pulsemcp.com`
- `mcpmarket.com`
- `cursor.directory/mcp`
- `mcp-get.com`
- `apify.com/store` MCP section
- `portkey.ai/mcp-servers`

**Agent-skill directories**
- `github.com/anthropics/skills` — discussions / community listing
- `github.com/topics/claude-skills` — tag the repo so it surfaces
- `awesome-claude-code` style lists (several; search `awesome claude code` on GitHub)
- `skills.directory` style aggregators as they appear — this category is new, so being
  early is cheap

**General AI-tool directories** (reuse the copy blocks already written for earlier
submissions)
- `alternativeto.net` — highest LLM-citation value; fill the "alternative to" field
- `theresanaiforthat.com`
- `futurepedia.io`
- `producthunt.com` — save for when the landing page and demo video are polished; one
  shot only
- `aitools.fyi`, `toolify.ai`, `futuretools.io`

**Submission copy**

> PUBLIC COPY
>
> **Name:** Moonya Social Autopilot (skill + MCP)
> **Tagline (≤60):** Put your social media on autopilot, from your AI agent
> **Short (≤160):** A free agent skill that plans, designs, schedules and publishes
> on-brand social posts across 9 networks through Moonya AI — you approve every one.
> **Long:** Moonya Social Autopilot is a free skill for AI agents. Install it, tell your
> agent what your business does, and it learns your brand voice and offer, plans a week
> of posts, designs the carousels, images and videos, writes a caption fitted to each
> network, and schedules everything at your best times across Instagram, Facebook,
> TikTok, YouTube, LinkedIn, X, Pinterest, Threads and Google Business. Every post is
> shown to you, slide by slide, in the chat — nothing is published without your
> approval, and you see the exact cost before anything is created. Works in Claude Code,
> Claude.ai, ChatGPT, Grok, Cursor, Codex and any MCP-capable agent. Runs on Moonya AI
> (7 days for $1).
> **Tags:** social media automation · AI agents · MCP · content creation · scheduling ·
> small business
> **Repo:** github.com/<org>/moonya-skill · **Site:** https://moonya.ai/en

---

## 4. Micro-creator tutorial brief

Target: creators with **2k–50k followers** who make AI-agent, no-code, automation or
small-business-marketing content. Small accounts have higher engagement per follower,
cost far less, and their audience trusts them more. One 7M-view Article from a 200-
follower account is the proof that reach here is uncorrelated with follower count.

**The brief to send them**

> **What to make:** a 3–8 minute video, thread or article where you put a real business
> account (yours, a friend's, a client's) on autopilot with your AI agent, from empty
> to a week of posts scheduled.
>
> **Must include**
> 1. The problem, in your own words, 20 seconds.
> 2. The install, on screen, unedited: sign up, add the connector, install the skill.
> 3. One real prompt and what came back — show the designed slides.
> 4. **The approval step.** Make it clear nothing publishes without the user. This is
>    the objection your audience will have, so answer it on screen.
> 5. The cost, said out loud: skill free, Moonya 7 days for $1, then $29/mo.
> 6. Your link, once, with the real prompt you used pinned in the description.
>
> **Must not include:** comparisons to other tools, invented results, "sponsored by"
> framing if it isn't, any claim we can't back. Show the failures too — a real session
> where something needed fixing converts better than a polished one.
>
> **What you get:** an extended Moonya account for the duration, help on camera if you
> want it, and <affiliate / revenue-share terms — decide before outreach>.
>
> **Assets:** repo link, landing page, logo, the 30-second demo clip, six example
> prompts.

**Outreach**: 20–30 creators, personal first lines referencing something they actually
made, one follow-up after five days, then stop. Track installs per creator with a
distinct landing URL each.

**Decide before any outreach**: affiliate terms (flat per paid conversion, or
percentage, and for how long) and how attribution is tracked. See `APP_CHANGES.md` —
there is no referral tracking today.

---

## 5. Measuring it

Without this you can't tell which of the seven moves worked, and you'll repeat the
wrong one.

- **Repo:** stars, clones, unique visitors per referrer (GitHub Insights → Traffic).
- **Landing:** visits and `Start` clicks, split by a per-channel URL parameter.
- **Trials:** signups attributed to the skill. **Needs the referral tracking in
  `APP_CHANGES.md` — build it before the Article.**
- **Activation:** of skill-sourced trials, how many connect the connector, complete the
  brand profile, and approve a first post. That funnel tells you whether to fix the
  skill or the marketing.
- **Which client:** the connector already logs the calling client per request — group
  skill-driven usage by it to see whether Claude Code, ChatGPT or Cursor users convert
  best, then write the next tutorial for that one.
- **Retention:** do skill-sourced accounts survive past the trial at the same rate as
  app-sourced ones? If not, the skill is overselling and the SKILL.md needs tightening.

---

## Copy rules — non-negotiable in anything public

1. **"7 days for $1."** Never "free trial", never "try it free". The skill is free; the
   product is $1 for 7 days.
2. **Always "Moonya AI"** on first mention. Never an abbreviation, never a nickname.
3. **No competitor names.** Not in the Article, not in tutorials, not in briefs, not in
   directory fields that aren't explicitly an "alternative to" field. No "better than",
   no "unlike X".
4. **No tech-stack vendor names.** The product is Moonya AI. Nothing about what it runs
   on, who hosts it, or whose models it uses. Agent *clients* (Claude Code, ChatGPT,
   Cursor…) are named only as install targets.
5. **No invented numbers.** No user counts, no "10x", no engagement lifts, no revenue
   claims, no testimonials we don't have. Real screenshots only.
6. **Approval is always mentioned.** "Nothing is published without your approval" is the
   line that beats the biggest objection — put it in every piece.
7. **One CTA per asset.**
8. **Prices from the product**: Starter $29 / Pro $79 / Agency $199 per month. If they
   change, this file and the landing page change the same day.

Moonya AI: https://moonya.ai/en
