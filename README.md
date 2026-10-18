# Glimind MCP server check

Audit your MCP server in CI: agent-readiness grade (schemas, descriptions, safety annotations, token cost, client compatibility), a tool-poisoning scan, and live protocol checks. The check performs the MCP `initialize` handshake and reads `tools/list`; it never calls your tools.

```yaml
name: MCP check
on: [push, pull_request]
jobs:
  glimind:
    runs-on: ubuntu-latest
    steps:
      - uses: glimind/check-action@v1
        with:
          target: https://your-server.example.com/mcp
          min-grade: B
```

For a server that only runs in CI, start it first and point `target` at `http://localhost:<port>/mcp`.

| Input | Default | Meaning |
|---|---|---|
| `target` | required | MCP endpoint URL, or a listed server id |
| `min-grade` | `C` | fail below this grade |
| `fail-on-safety` | `danger` | fail on tool-poisoning findings at this level (`review`, `danger`, `never`) |
| `fixes-file` | none | write corrected tool definitions to this path |

Outputs: `grade`, `score`. Exit codes: 0 pass, 1 below threshold, 2 could not check.

Every finding links to a fix guide at https://glimind.com/guide/fix. Full report for any listed server: https://glimind.com/server/<server id>. Same check without CI: `pipx run glimind check <url>` or https://glimind.com/check.

MIT licensed.
