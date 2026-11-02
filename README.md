# HLN Apparel — clothing manufacturer (MCP server)

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for [HLN Apparel](https://www.hlnapparel.com), a China clothing factory. It lets AI assistants look up garment categories, MOQ, lead times, US HS codes and duty rates, convert fabric weight (GSM ⇄ oz/yd²), and send buyer-confirmed quote requests.

| | |
|---|---|
| Endpoint | `https://www.hlnapparel.com/api/mcp` |
| Transport | Streamable HTTP |
| Auth | None |
| Official MCP Registry name | `com.hlnapparel/clothing-manufacturer` |
| Website | https://www.hlnapparel.com |
| Free buyer tools (web) | https://www.hlnapparel.com/tools |

> Note: the endpoint only answers MCP clients. Opening it in a browser (GET) returns 405 — that is expected.

## Tools

| Tool | Read-only | Description |
|---|---|---|
| `get_factory_overview` | yes | Who HLN Apparel is: founding year, quality standard, audits, shipping terms, scale and key page links. Optional `language`: en, es, fr. |
| `list_product_categories` | yes | Garment categories HLN makes, each with typical fabrics, GSM range, MOQ, sample time, bulk lead time, size range, customisation options and the category page URL. Optional `language`: en, es, fr. |
| `lookup_hs_codes` | yes | Suggested HS codes for garments, the US HTS line, the US general duty rate and the total rate for goods made in China (Section 301 plus other China tariffs), with the CBP ruling each row is based on. Optional `group` (t-shirts, hoodies, polos, activewear, …). |
| `convert_fabric_weight` | yes | Convert fabric weight between GSM and ounces per square yard, and list garments typically made at that weight. |
| `preview_inquiry` | yes | Step 1 of 2. Checks a quote request and returns a summary plus a `confirmation_token`. Sends nothing. |
| `submit_inquiry` | **no** | Step 2 of 2. Sends the confirmed quote request to the HLN sales team and emails the buyer a receipt. |

⚠️ **`submit_inquiry` sends a real inquiry** to the HLN sales team and a receipt email to the buyer. Only call it after the buyer explicitly confirmed the `preview_inquiry` summary. Please do not use it for testing.

## Setup

### Claude Desktop / Claude.ai
Settings → Connectors → Add custom connector → URL `https://www.hlnapparel.com/api/mcp`.

Or in `claude_desktop_config.json` via [mcp-remote](https://www.npmjs.com/package/mcp-remote):
```json
{
  "mcpServers": {
    "hln-apparel": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://www.hlnapparel.com/api/mcp"]
    }
  }
}
```

### Cursor
`~/.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "hln-apparel": { "url": "https://www.hlnapparel.com/api/mcp" }
  }
}
```

### VS Code (GitHub Copilot)
`.vscode/mcp.json`:
```json
{
  "servers": {
    "hln-apparel": { "type": "http", "url": "https://www.hlnapparel.com/api/mcp" }
  }
}
```

### ChatGPT
Settings → Apps & Connectors → Advanced → enable Developer mode → Create connector → MCP server URL `https://www.hlnapparel.com/api/mcp`, authentication: none.

### Quick test (curl)
```bash
curl -s -X POST https://www.hlnapparel.com/api/mcp \
  -H 'Content-Type: application/json' -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## About HLN Apparel
Website: https://www.hlnapparel.com · Free buyer tools: https://www.hlnapparel.com/tools

This repository contains documentation and the public `server.json` only; the server is hosted by HLN Apparel.

## License
MIT
