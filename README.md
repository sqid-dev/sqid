<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/logo-dark-mode.png">
    <img src=".github/assets/logo-light-mode.png" height="56" alt="SQID">
  </picture>
</p>

<h2 align="center">Give your agent eyes.</h2>

<p align="center">
  SQID is the open-source visitor runtime for AI agents.<br>
  It streams what each website visitor is doing, and what they're likely to do next,<br>
  into your agent <b>before it says a word.</b>
</p>

<p align="center">
  <a href="https://docs.sqid.dev">Docs</a> ·
  <a href="#mcp">MCP server</a> ·
  <a href="#frameworks">Framework tools</a> ·
  <a href="https://sqid.dev">SQID Cloud</a> ·
  <a href="https://github.com/sqid-dev/sqid/discussions">Discussions</a>
</p>

---

> **Early access.** We're opening the SQID runtime and building the agent tools in public.
> **Star the repo** to follow along. Tell us what your agent would do with visitor context in [Discussions](https://github.com/sqid-dev/sqid/discussions).

## Your agent knows the conversation. It doesn't know the visitor.

Every chat agent, AI SDR and onboarding bot starts the same way:

> **Visitor:** do you have an enterprise plan?
> **Agent:** Great question! We offer several plans. Here's an overview…

The visitor has been on pricing for four minutes, compared Team vs. Enterprise twice and read your SSO docs. Your agent has no idea.

**With SQID**, the agent gets this before it answers:

```json
{
  "summary": "3rd visit this week. Viewed pricing 4x, compared Team vs Enterprise, read SSO + SCIM docs.",
  "intent":  { "stage": "evaluating", "score": 0.82, "topic": "enterprise, SSO" },
  "source":  "google / \"acme sso pricing\"",
  "company": { "name": "Northwind", "size": "500-1000", "industry": "Logistics" },
  "person":  null
}
```

> **Agent:** Yes. Enterprise includes SSO and SCIM. Most teams Northwind's size start with a security review call. Want me to set one up?

Same model, same question, a much better answer.

`person` is `null` because this site hasn't turned on identification. **SQID is anonymous by default.** Identity is something you switch on, not something you get by accident.

## Three ways your agent uses SQID

| | How | Best for |
|---|---|---|
| **Pull** | The agent calls `visitor_context` as a tool whenever it needs it | Chat agents, support bots, copilots |
| **Push** | SQID tells the agent when something happens: *"intent crossed 0.8 on /pricing"* | AI SDRs, proactive chat, alerts |
| **In-page** | Your widget listens to the SDK in the browser and passes context straight into the prompt | Website builders, embedded widgets |

```
 Visitor's browser                SQID runtime                           Your agent
 ─────────────────                ────────────                           ──────────
 SDK ──live events──▶  sessions · key events · intent ──┬── pull ──▶  visitor_context (tool / MCP)
                                                         ├── push ──▶  webhook · Kafka · Pulsar
                                  (optional) identity    └── in-page ▶ squid.engine() events
                                  via SQID Cloud key
```

<a id="mcp"></a>
## MCP server *(planned)*

Plug SQID into Claude, ChatGPT, Cursor or any MCP client:

```json
{
  "mcpServers": {
    "sqid": {
      "command": "npx",
      "args": ["-y", "@sqid-dev/mcp"],
      "env": { "SQID_HOST": "http://localhost:4000" }
    }
  }
}
```

| Tool | What it returns |
|---|---|
| `live_visitors` | Who's on the site right now, with intent |
| `visitor_context` | A short, LLM-ready summary of one visitor: pages, intent, source, and company and person if enabled |
| `visitor_journey` | The full timeline of a visitor's sessions |
| `subscribe_intent` | Push notifications when a visitor crosses an intent threshold |

<a id="frameworks"></a>
## Drop-in tools for your agent framework *(planned)*

```ts
import { sqidTools } from '@sqid-dev/ai';          // Vercel AI SDK
import { generateText } from 'ai';

const { text } = await generateText({
  model,
  tools: sqidTools({ host: process.env.SQID_HOST }),
  system: 'Use visitor_context before answering a website visitor.',
  prompt: userMessage,
});
```

Also planned: **OpenAI Agents SDK**, **LangChain**, **Mastra** and **Claude Agent SDK**. [Tell us which one you need first.](https://github.com/sqid-dev/sqid/discussions)

## What people build with it

| | |
|---|---|
| **Website chat that skips the intro** | The agent opens with what the visitor came for, not "How can I help?" |
| **AI SDRs with a reason to reach out** | Follow up on *what they read*, not "saw you visited our site". |
| **Agents that personalize the page** | Let your agent rewrite the hero, pricing or CTA for this visit. |
| **Onboarding copilots** | Know where a new user got stuck before they ask. |
| **Hesitation saves** | Step in when intent drops, and stay quiet when they're just reading. |
| **Know when the visitor *is* an agent** *(experimental)* | Spot AI browsers and agents on your site, and see what they read. |

## Open source vs. SQID Cloud

Everything your agent needs to see behavior and intent is open source and self-hostable.
**Identity and enrichment** run on licensed data, so they're a paid API you switch on with a key. **Your code doesn't change either way.**

| | **Open source** | **SQID Cloud** |
|---|:---:|:---:|
| Browser SDK and real-time engine | Yes | Yes |
| Intent scoring, key events, journeys | Yes | Yes |
| `visitor_context` API + MCP server + framework tools | Yes | Yes |
| Webhooks, Kafka, Pulsar and other sinks | Yes | Yes |
| Self-host with Docker | Yes | n/a |
| **Identity on demand**: person + company, 200+ fields | | Yes |
| **Enrichment** by email, phone or LinkedIn | | Yes |
| Hosted, scaled, monitored | | Yes |
| **OEM** for agent platforms: per-customer SDKs, white-label | | Yes |

### Building an agent product?

If you're building a chat, SDR or website agent **for other companies**, SQID Cloud gives each of your customers an isolated SDK through the API, so "our agent knows your visitors" becomes **your** feature. [Talk to us →](https://sqid.dev/#oem)

## Quickstart *(planned)*

```bash
git clone https://github.com/sqid-dev/sqid && cd sqid
cp .env.example .env
docker compose up                      # engine + gateway at http://localhost:4000
```

```html
<script async src="http://localhost:4000/sdk.js" data-sdk="YOUR_SDK_ID"></script>
```

```js
const engine = new squid.engine();
engine.on('visitor.intent', ({ detail }) => {
  if (detail.score > 0.8) agent.open(detail.summary);   // hand the moment to your agent
});
```

Turn on identity later with one line in `.env`: `SQID_CLOUD_KEY=sk_live_...`

## Built for agents that people trust

- **Anonymous by default.** No identity unless the site owner turns it on.
- **Nothing personal reaches the LLM unless you allow it.** `context.pii: false` strips names and emails before the agent sees them.
- **Consent-ready.** `lazy: true` waits for your consent manager.
- **Use it, don't announce it.** Our guidance for agents: use context to give a better answer, and don't recite what you know.

## Roadmap

- [ ] `visitor_context` API and MCP server
- [ ] `@sqid-dev/ai` tools for Vercel AI SDK, OpenAI Agents SDK, LangChain, Mastra
- [ ] Self-hosted engine + Docker Compose
- [ ] "Website sales agent" starter template
- [ ] Push intent to agents (webhook, Kafka, Pulsar)
- [ ] AI-agent visitor detection *(experimental)*

The roadmap follows what you ask for. [Start a discussion →](https://github.com/sqid-dev/sqid/discussions)

## License

- SDK, MCP server, framework tools, connectors and examples: **MIT**
- Engine and gateway: **AGPL-3.0**. Free to self-host and modify. If you run it as a hosted service, share your changes.
- Identity and enrichment aren't in this repo. They're available through [SQID Cloud](https://sqid.dev).
