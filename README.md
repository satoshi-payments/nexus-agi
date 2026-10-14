# NEXUS AGI

**Intelligence for your AI agents: the critical context, at the moment they decide.**

So your agent doesn't repeat a costly mistake. NEXUS gathers what all your agents learn (Claude, ChatGPT, Cursor and any MCP agent) and hands back what matters before the agent acts: your decisions, your business rules and what already went wrong, in this session and in the one three months from now. On Claude Code and Cursor it arrives on its own through hooks; in chat clients the agent asks for it.

- Website: https://nexus.eblas.link · [Español](https://nexus.eblas.link)
- Remote MCP server: `https://nexus.eblas.link/mcp` (Streamable HTTP, OAuth 2.1)
- Official MCP Registry: `link.eblas.nexus/nexus-agi`
- Free plan: 100 writes and 2 agents a month, no credit card. [Create an account](https://nexus.eblas.link/en/signup)

> This repository holds the public documentation and connection examples. NEXUS itself is a hosted service; nothing needs to be installed to use it.

## Why

An agent's costly mistakes are rarely about intelligence:

- **It contradicts something you already decided.** The decision was made three sessions ago; today's agent wasn't there.
- **It repeats a mistake you already fixed.** You fixed it with Claude; tomorrow Cursor makes it again, because every tool remembers on its own.
- **It acts on a fact that stopped being true.**

NEXUS gives all your agents one shared source of what matters: decisions, rules, mistakes and what changed, each marked with how it is known and whether it is still valid.

*Illustrative example:* you decide in Claude Code that payment records are archived, never deleted, because accounting audits them. Days later, in Cursor, you ask to "clean up payments older than a year". Cursor recalls the decision from NEXUS and proposes moving them to an archive table instead of running a `DELETE`.

## Connect

### Claude (web, desktop, mobile)

1. Open **Customize → Connectors** and add a custom connector.
2. URL: `https://nexus.eblas.link/mcp`
3. Click **Connect**, sign in with the code sent to your email, review the permissions and authorize.

### Claude Code

```bash
claude mcp add --transport http nexus-agi https://nexus.eblas.link/mcp
```

Then run `/mcp` inside Claude Code to authorize in the browser.

### ChatGPT

1. **Settings → Apps → Advanced settings**, turn on **Developer mode**.
2. Create a new app named `Nexus AGI`, URL `https://nexus.eblas.link/mcp`, OAuth authentication.
3. Sign in, review the permissions and authorize.

### Cursor and other MCP clients

Create an agent credential (`nxa_…`) in the NEXUS console, then add to `.cursor/mcp.json` (or `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "nexus-agi": {
      "type": "http",
      "url": "https://nexus.eblas.link/mcp",
      "headers": { "Authorization": "Bearer nxa_YOUR_CREDENTIAL" }
    }
  }
}
```

### The instructions that make chat clients use it

Claude and ChatGPT have no hooks, so nothing recalls memory on the agent's behalf. A concrete order works where a judgment call doesn't. Paste this at the top of the agent's instructions:

```text
Your long-term memory is NEXUS (connector "Nexus AGI").
Every turn, before answering, call retrieve_memories with my message as the query and time_range="all_time".
If a result carries duda_de_fusion, decide it with decide_merge_proposal before answering; don't ask me about it.
When you learn something you would want to know next time (a decision, a preference, a mistake), save it with process_observation in that same turn.
```

On Claude Code and Cursor, NEXUS offers optional hooks that recall memory when a session starts and save what matters when it ends. Your agent can fetch the installer with the `get_setup_kit` tool; nothing is installed unless you approve it.

## What it exposes

Read-only tools are annotated as such, writes as writes, and tools that overwrite or delete data as destructive, so your client asks before using them.

- **Memory:** store and recall episodes, facts and procedures; mark a memory as no longer valid without deleting it.
- **Graph:** entities (people, projects, systems) and their relations, with validity over time.
- **Rules and hypotheses:** rules the agent should follow; hypotheses with their evidence.
- **Goals and tasks:** create, update and close them.
- **Agent messaging:** your agents can leave messages for each other.

Authentication is OAuth 2.1 with dynamic client registration and PKCE (S256). Scopes: `memoria` (memory), `grafo` (graph), `cognicion` (cognition), `trabajo` (work).

## Known limits

- NEXUS knows what it is told. Without hooks, it only has what the agent stores on purpose.
- A memory can become outdated. NEXUS tracks validity and lets you replace memories, but it cannot tell on its own that something in the world changed.
- Automatic conclusions (hypotheses, suggested relations) can be wrong and are presented with their evidence.
- Beta: tools may change and there may be maintenance windows.

## Privacy

Each account is an isolated tenant. NEXUS does not read your chat history or the built-in memory of Claude or ChatGPT; it only receives what your agent sends through its tools. We do not sell your data or use it to train third-party models. [Privacy notice](https://nexus.eblas.link/en/privacy) · [Terms](https://nexus.eblas.link/en/terms)

## Support

info@eblas.link · [Documentation](https://nexus.eblas.link/en/docs)

---

NEXUS by eblas · Satoshi Payments, S.A.S. de C.V. · Mexico
