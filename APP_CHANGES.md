# Proposed changes to the Moonya app

**Nothing here is implemented.** No change was made to the `vajbpilot` working tree,
`main` was not touched and nothing was deployed. These are proposals for Nemanja to
approve, ordered by what blocks the launch first.

Each item names the files it would touch so it can be handed to an agent as-is.

---

## P0 — Blocks the launch

### 1. Skill landing page at `/en/skill/`

**Why:** every link in `MARKETING.md` points here — the X Article, the directory
listings, the creator tutorials. There is `/docs/mcp/` (developer-facing) and
`/integrations/<client>/` (per-client connector pages), but no page that sells the
*skill* to a business owner.

**What:** a new static page, same build pattern as `/integrations/index.html`, content
per the outline in `MARKETING.md §1`.

**Touches**
- new `en/skill/index.html` (+ a Serbian twin if the other locales get one)
- `sitemap.xml` — add the URL
- `llms.txt` — add a line so AI search can cite it
- `docs/mcp/index.html` — link "Install the skill" near the top
- `integrations/index.html` and each `integrations/<client>/` — one card linking to the
  skill page
- reuse `app/landing-mcp-motion.css|js` if the hero animation fits

**Also needed:** a `/skill` → `/en/skill/` redirect, since that is the short URL people
will type and that will get written in tutorials.

### 2. Referral attribution for skill-sourced signups

**Why:** right now there is no way to tell that a trial came from the skill. Without it,
none of the seven marketing moves can be judged against each other and the creator
affiliate programme can't pay out. This has to exist **before** the Article goes out,
not after — retrofitted attribution loses the spike it was meant to measure.

**What (minimum viable):**
1. The landing page reads a `?ref=` parameter (e.g. `?ref=skill`, `?ref=xarticle`,
   `?ref=creator-<handle>`), stores it in a first-party cookie or `localStorage`, and
   carries it through sign-up.
2. Persist it on the workspace on creation — one nullable `signup_ref text` column.
3. Surface it in whatever internal reporting already reads signups, grouped by `ref`.

**What it is not:** not a full affiliate system, not a payout engine. One column and a
query. The payout decision can stay manual until volume justifies more.

**Touches:** `en/skill/index.html`, the sign-up path in `app/`, the workspace-creation
route in `workers/src/routes/`, one Supabase migration. Note the post-30.10.2026 rule:
any new table needs explicit `grant` statements in the migration — a new *column* on an
existing table does not, which is a reason to prefer the column.

**Verify before building:** check whether a UTM or referral field is already captured
anywhere in the sign-up flow. If one exists, extend it instead of adding a column.

---

## P1 — Materially improves conversion from install → trial

### 3. Deep link that opens the API-key dialog with the client preselected

**Why:** the slowest step in the whole install is "go to Settings → Account → API keys,
create a key, pick your client". The agent knows which client it is running in (the MCP
`clientInfo.name` is already received), so it should be able to hand over a link that
lands directly on the right form.

**Current state:** the key form in `app/hq/hq-settings.js` has a client `<select>` and an
"All my brands" toggle, but no URL parameter opens it or preselects a client. The
existing deep link only reaches the settings tab:
`/app/hq/?account=…&view=settings&settings=account`.

**What:** support `&connect=<client-id>` on that link — open the create-key form and
preselect the client. Client ids already exist in `app/mcp-clients.js`
(`claude-code`, `codex`, `cursor`, `vscode`, `gemini-cli`, …).

**Then:** add a `connect(client)` link to `buildLinks()` in `workers/src/lib/mcp-text.js`
so `moonya_links` and the skill can use it.

**Touches:** `app/hq/hq-settings.js`, `workers/src/lib/mcp-text.js`.

### 4. A `skill` block on `moonya_capabilities`

**Why:** a skill installed from a repo goes stale silently. If the connector reports the
current public skill repo and version, an outdated copy can tell the user to update —
and the agent can point a user who has the connector but *not* the skill at the repo,
which is a free install channel from inside the product.

**What:** add to the `moonya_capabilities` structured result:

```json
"skill": {
  "repo": "https://github.com/<org>/moonya-skill",
  "landing": "https://moonya.ai/en/skill",
  "version": "1.0.0",
  "skills": ["moonya-social-autopilot", "moonya-clip-to-posts", "moonya-weekly-report"]
}
```

Source the values from `lib/product-knowledge.js` so they stay in one place, as that
file already is the single source of truth for MCP instructions and the support
assistant.

**Touches:** `workers/src/lib/product-knowledge.js`,
`workers/src/lib/mcp-tools.js` (`moonya_capabilities`), and the
`scripts/test-product-knowledge.mjs` expectations.

### 5. Surface the skill to users who already have the connector

**Why:** existing connector users are the warmest possible audience for the skill, and
they cost nothing to reach.

**What:** one line on the API-keys screen under a created key — *"Using Claude Code or
Codex? Install the Moonya skill so your agent runs the whole workflow →"* — plus the
same link in the MCP onboarding copy.

**Touches:** `app/hq/hq-settings.js`, the relevant i18n strings.

---

## P2 — Worth doing, not blocking

### 6. Tool-side gaps found while writing the skills

The MCP surface is genuinely complete for everything the three skills need — all 60+
tools used were verified against `workers/src/lib/mcp-tools.js` and the live connector.
Three small things came up:

- **`moonya_auto_batch` plan slots.** Calling it without `confirm` returns the plan mix
  and the credit estimate, which is what the skill needs to quote a price. It would help
  to also return the **per-slot dates, formats and working titles** in that free
  response, so the agent can show the user the actual week as a table *before* charging
  rather than a mix summary. **Verify** against the current implementation — it may
  already include this.
- **Best time per network.** `moonya_analytics_summary` and `moonya_status` return a best
  posting time. If that is a single workspace-wide time rather than one per network, a
  per-network breakdown would improve scheduling, since Instagram and LinkedIn audiences
  are rarely active at the same hour. **Verify before building.**
- **Skill-sourced usage reporting.** The `mcp_tool_call` audit log already records the
  client per call. A grouped read of it (calls and workspaces per client, per week)
  would answer "which agent client converts best" directly, which decides who the next
  tutorial is aimed at. This is a reporting query, not a schema change.

### 7. Docs cross-links

`docs/MCP-INTEGRACIJA.md` is the internal source of truth for the connector and is
accurate. Add a short section pointing at the public skill repo and the landing page so
the next person working on the connector knows the skill exists and keeps it in sync
when tools change.

---

## Explicitly out of scope

- No changes to `vajbpilot` `main`. Nothing committed to the app repo by this work.
- No deploys.
- No changes to credit prices, plans or the trial. All copy in this repo uses the
  current values (post 3 · AI image 5 · caption 1 · site analysis 5 · X publish 2;
  Starter $29 / Pro $79 / Agency $199; **7 days for $1**) read from
  `workers/src/lib/plan.js` and `workers/src/lib/product-knowledge.js`. If any of those
  change, the skills, README and MARKETING copy change with them.
- No new MCP tools. The skills deliberately use only what already ships, so they work
  against the live connector today.

Moonya AI: https://moonya.ai/en
