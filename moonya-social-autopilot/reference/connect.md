# Connect Moonya AI to your agent

The skill needs the Moonya AI MCP connector. Two ways in: a **key** (coding agents and
CLIs) or **sign-in in a window** (OAuth — chat clients).

Server URL: `https://moonya.ai/mcp`

## 0. Get a Moonya account

Start at **https://moonya.ai/en** — **7 days for $1**, then Starter $29 / Pro $79 /
Agency $199 per month.

## 1. Get an API key (key-based clients only)

In Moonya: **Settings → Account → API keys → New key** → pick your client → copy the
key. It looks like `mk_live_…` and **it is shown only once**.

- The key belongs to **one workspace** and only the workspace owner can create one.
- Turn on **"All my brands"** when you want one key to work across every brand you
  manage; then use `moonya_brands` / `moonya_use_brand` in the conversation.
- Keys cannot touch billing, identity or admin routes. Max 10 active keys.
- Revoke a key any time from the same screen.

Never paste a key into a public repo, a shared transcript, or a chat with anyone else.

## 2. Add the connector

### Claude Code

```bash
claude mcp add --transport http moonya https://moonya.ai/mcp \
  --header "Authorization: Bearer mk_live_…"
```

### Claude.ai (web and desktop)

Settings → **Connectors** → *Add custom connector* → URL `https://moonya.ai/mcp` →
**Connect**. A Moonya page opens: sign in, choose the workspace, **Allow**. No key to
type.

### ChatGPT

Settings → **Connectors** (Developer mode / Apps) → *Create* → URL
`https://moonya.ai/mcp` → sign in and allow, same as above.

### Grok

Add a custom MCP connector with URL `https://moonya.ai/mcp` and sign in when the
Moonya page opens.

### Cursor

`.cursor/mcp.json` (or the global `mcp.json`):

```json
{ "mcpServers": { "moonya": {
  "url": "https://moonya.ai/mcp",
  "headers": { "Authorization": "Bearer mk_live_…" }
} } }
```

### Codex

`~/.codex/config.toml`:

```toml
[mcp_servers.moonya]
url = "https://moonya.ai/mcp"
http_headers = { Authorization = "Bearer mk_live_…" }
```

### Gemini CLI

```json
{ "mcpServers": { "moonya": {
  "httpUrl": "https://moonya.ai/mcp",
  "headers": { "Authorization": "Bearer mk_live_…" }
} } }
```

### VS Code, Windsurf, Zed, Perplexity, Manus, Raycast, LM Studio, Kimi, and others

Same two values everywhere — a **streamable HTTP MCP server** at
`https://moonya.ai/mcp` and an **`Authorization: Bearer mk_live_…`** header. Moonya's
API-keys screen prints the exact snippet for each supported client, so copy it from
there if your client is not listed above.

## 3. Verify

Ask your agent: **"What can Moonya do for my brand?"**

It should call `moonya_capabilities` and `moonya_status` and come back with your plan,
credits and connected networks. If instead you get an auth error, the key is wrong,
revoked, or belongs to a different workspace — create a new one.

Manual check:

```bash
curl -s https://moonya.ai/mcp \
  -H "Authorization: Bearer $MOONYA_KEY" \
  -H "content-type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## 4. Connect your social networks

Connecting Instagram, Facebook, TikTok, YouTube, LinkedIn, Pinterest, Threads, X and
Google Business happens **in the browser** — it is an account-permission step no agent
can do for you. Ask the agent for the link (`moonya_links`) and it will hand you the
right one.

Drafts work without a connected network. A network is only needed to publish or
schedule, and for automations to run on their own.

## What stays in the browser

Sign-in, payment and plan changes, connecting networks, uploading a video from a chat
client with no shell access, TikTok privacy settings and Pinterest boards. Everything
else — Brain, planning, generation, editing, approval, scheduling, analytics, inbox —
works from the chat.

Moonya AI: https://moonya.ai/en
