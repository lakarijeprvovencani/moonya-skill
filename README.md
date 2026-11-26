# Moonya AI skills for AI agents

Three agent skills that put a real business's social media on autopilot — planned,
designed, scheduled and published across **9 networks** — from inside Claude Code,
Claude.ai, ChatGPT, Grok, Cursor, Codex and any other MCP-capable agent.

Your agent does the talking. [**Moonya AI**](https://moonya.ai/en) does the work:
it holds the brand's voice, designs the slides, renders the videos, keeps the calendar,
publishes to the networks and collects the numbers.

```
"Plan and schedule next week's posts for my shop."
→ reads your brand → plans 4 posts → designs them → shows you every slide
→ you approve → scheduled at your best times on Instagram, TikTok, Facebook and LinkedIn
```

## The skills

| Skill | What it does |
|---|---|
| **`moonya-social-autopilot`** | The whole job: learn the business, plan a week or month, generate carousels / images / Reels, schedule at the best times, approval gate before anything goes live, then report the links. |
| **`moonya-clip-to-posts`** | One clip → every network. Upload, burn in branded captions, pick a cover frame that isn't black, write a caption per platform, schedule as Reel + TikTok + Short + Story + feed post. |
| **`moonya-weekly-report`** | The numbers, then the decisions. Reach and engagement per network, best times, top posts, comment-to-DM results → next week scheduled before the session ends. |

Networks: Instagram · Facebook · TikTok · YouTube · LinkedIn · X · Pinterest · Threads ·
Google Business.

## Install in 1 minute

### 1. Get Moonya AI

Sign up at **[moonya.ai/en](https://moonya.ai/en)** — **7 days for $1**, then Starter
$29 / Pro $79 / Agency $199 per month.

### 2. Connect the Moonya connector to your agent

**Claude Code** — create a key in Moonya (*Settings → Account → API keys*), then:

```bash
claude mcp add --transport http moonya https://moonya.ai/mcp \
  --header "Authorization: Bearer mk_live_…"
```

**Claude.ai** (web / desktop) — Settings → **Connectors** → *Add custom connector* →
`https://moonya.ai/mcp` → **Connect** → sign in and allow. No key to type.

**ChatGPT** — Settings → **Connectors** (Developer mode / Apps) → *Create* →
`https://moonya.ai/mcp` → sign in and allow.

**Grok** — add a custom MCP connector pointing at `https://moonya.ai/mcp` and sign in.

**Cursor** — `.cursor/mcp.json`:

```json
{ "mcpServers": { "moonya": {
  "url": "https://moonya.ai/mcp",
  "headers": { "Authorization": "Bearer mk_live_…" }
} } }
```

**Codex** — `~/.codex/config.toml`:

```toml
[mcp_servers.moonya]
url = "https://moonya.ai/mcp"
http_headers = { Authorization = "Bearer mk_live_…" }
```

Anything else (VS Code, Windsurf, Zed, Gemini CLI, Perplexity, Manus, Raycast,
LM Studio, Kimi…) takes the same two values — a streamable HTTP MCP server at
`https://moonya.ai/mcp` and an `Authorization: Bearer mk_live_…` header. Moonya's
API-keys screen prints the exact snippet for each one.

### 3. Install the skills

**Claude Code** — drop them in your skills directory:

```bash
git clone https://github.com/lakarijeprvovencani/moonya-skill.git
cp -r moonya-skill/moonya-* ~/.claude/skills/
```

Per-project instead of global: copy into `.claude/skills/` in the repo.

**Claude.ai** — Settings → **Capabilities** → **Skills** → upload each skill folder
(or a zip of it).

**Any other agent** — the skills are plain Markdown with YAML frontmatter. Point your
agent at `moonya-social-autopilot/SKILL.md`, or paste it in as instructions. The
`reference/` files are loaded on demand.

### 4. Connect your social accounts

Connecting Instagram, Facebook, TikTok and the rest happens in the Moonya app — it is
an account-permission step no agent can do for you. Ask your agent for the link and it
will hand you the right one.

Drafts work without a connected network. A network is only needed to publish.

## Try it

```
Put my Instagram and TikTok on autopilot.
```
```
Plan and schedule next week's posts for my business.
```
```
Make a carousel about why we make everything to order, and schedule it for Wednesday at 6pm.
```
```
I filmed a 30-second clip. Add captions and post it everywhere.
```
```
How did my posts do last week, and what should I post next?
```
```
Set up a comment-to-DM so people who comment PRICE get my price list.
```
```
Read my website and fill in my brand profile, then make the first post.
```

## What your agent will and won't do

**It will**

- Learn the brand once — what it sells, who buys, the tone, the colours, the rules — and
  keep it. Corrections you make are saved permanently, not just for the post in front
  of you.
- Plan with a mix, not just product posts, and schedule inside the real limits (at least
  60 minutes between posts on the same network, max 3 a day per network).
- Show you **every slide as an image, right in the chat**, before anything is scheduled.
- Tell you the **exact credit cost** before it spends anything, and wait for your yes.
- Write a caption fitted to each network — X's 280 characters, Instagram's 5-hashtag
  limit, LinkedIn's tone.
- Report what actually happened, including partial failures.

**It won't**

- Publish anything you haven't approved.
- Spend credits without asking first.
- Invent a statistic, a testimonial, a price or a post link.
- Claim a post is live because it called a tool. It reads the real status.

## What needs a browser

Sign-in, payment and plan changes, connecting social networks, uploading a video from a
chat client with no shell access, TikTok privacy settings and Pinterest boards.
Everything else — brand profile, planning, generation, editing, approval, scheduling,
analytics, inbox — happens in the chat.

## Requirements

- An [**Moonya AI**](https://moonya.ai/en) account (7 days for $1) with the MCP
  connector added to your agent.
- An agent that supports MCP and skills, or any agent you can give Markdown
  instructions to.

Without the Moonya connector these skills do nothing — Moonya is what holds the brand,
renders the designs and talks to the networks.

## License

MIT — see [LICENSE](LICENSE). Use them, fork them, ship them in your own stack.

---

**[Moonya AI](https://moonya.ai/en)** — put your social media on autopilot.
7 days for $1.
