---
title: Enable shadcn MCP server
impact: MEDIUM
impactDescription: Let the agent browse/install shadcn components via MCP
tags: setup, mcp, shadcn
---

## Enable shadcn MCP server

Configure the shadcn MCP server so the agent can browse/search/install components directly from registries (default shadcn/ui, plus any custom registries).

### Steps

1) Init MCP for Cursor (example):
```bash
npx shadcn@latest mcp init --client cursor
```

2) Add config (Cursor example):
```json
// .cursor/mcp.json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

3) Ensure `components.json` lists registries if you use custom ones:
```json
{
  "registries": {
    "@acme": "https://acme.com/r/{name}.json"
  }
}
```

4) Restart the client and verify the shadcn MCP server is connected.

5) Example prompts (for the agent):
- “Show available components in the shadcn registry.”
- “Add button, dialog, card.”
- “Find a login form component.”

### Why this matters

- Enables structured component discovery and installation instead of manual copy/paste.
- Keeps the migration aligned with shadcn’s canonical implementations.

### Gotchas

- Ensure network access and registry auth (if private registries) are configured via env vars.
- Keep `components.json` in sync with desired registries/namespaces.
