# Configuration

## Env and `.env`

Re-read on every `shapeshift` / `call` — add keys mid-session, no restart.

Search order: `CWD/.env` → `~/.env` → `~/.kitsune/.env` (last wins).

```bash
auth("BRAVE_API_KEY", "sk-...")    # → ~/.kitsune/.env
```

## Tool surface

```json
{ "env": { "KITSUNE_TOOLS": "shapeshift,call,auth" } }   # subset
{ "env": { "KITSUNE_TOOLS": "all" } }                    # forge
```

## State directory

Default `~/.kitsune/` (credentials, pins, OAuth, session). Relocate with `KITSUNE_HOME=/tmp/kitsune-iso`.

## Sandbox / trust policy

```bash
KITSUNE_SANDBOX=community   # Docker-cage community npm/PyPI mounts
KITSUNE_SANDBOX=all         # cage every local mount
KITSUNE_TRUST=community     # waive confirm gate (status warns)
KITSUNE_REPIN=1             # adopt newer pinned version
```

## Smithery (removed)

Smithery shut down its API, Connect proxy and hosted `server.smithery.ai` /
`run.tools` URLs on 2026-10-31, so Kitsune no longer uses it. `SMITHERY_API_KEY`
is ignored. Search covers the official registry, MCP Registry, Glama, npm, PyPI
and GitHub without any key; to use a hosted server, pass its own `https://` MCP
URL to `shapeshift()` or `call()`.

## GATEWAY: consolidate always-on servers

Optional. Keep daily drivers (GitHub, filesystem, …) native if you prefer. When a config is crowded, `status()` flags other always-on servers so you can collapse to one Kitsune entry and reach them via `shapeshift`:

```
GATEWAY
  ⚠  1 other server(s) active in claude-desktop (~8 extra tools in context)
     Run setup() to harvest their credentials and reduce bloat
```

```python
setup()  # preview
setup(action="harvest")  # keys → ~/.kitsune/.env (non-destructive)
setup(action="absorb")  # register for shapeshift()
setup(project=True)  # project mcp.json with only Kitsune
```

Never modifies existing configs without explicit confirmation. (`setup` is forge-profile.)

## Mount patterns

Switch forms mid-session — take only the slice you need:

```python
# Research
shapeshift("brave", tools=["brave_web_search"])
shapeshift("mcp-server-fetch")
shapeshift("@modelcontextprotocol/server-memory", tools=["read_graph", "search_nodes"])

# Code
shapeshift(
    "@modelcontextprotocol/server-filesystem",
    tools=["read_file", "write_file", "edit_file"],
    server_args=["/path/to/project"],
)
shapeshift("mcp-server-git", tools=["git_status", "git_diff", "git_log"])

# Notes
shapeshift("notion-hosted", tools=["notion-search", "notion-append-block-children"])
shapeshift("@modelcontextprotocol/server-memory", tools=["add_memory", "search_nodes"])

shapeshift()  # always drop when the task is done
```

---

[← README](https://github.com/kaiser-data/kitsune-mcp#readme)
