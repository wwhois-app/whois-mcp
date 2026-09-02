# wwhois — MCP server

WHOIS/RDAP domain lookup, IP geolocation and Punycode conversion, exposed as
[Model Context Protocol](https://modelcontextprotocol.io) tools. Backed by
[wwhois.ru](https://wwhois.ru).

- **Server URL:** `https://wwhois.ru/backend/mcp`
- **Transport:** Streamable HTTP (JSON-RPC 2.0)
- **Auth:** none — public, read-only
- **Rate limit:** 30 tool calls / minute per client IP
- **Docs:** https://wwhois.ru/mcp

## Connect

Claude Desktop, Cursor, and other clients with an MCP server config:

```json
{
  "mcpServers": {
    "wwhois": { "type": "http", "url": "https://wwhois.ru/backend/mcp" }
  }
}
```

## Tools

| Tool | Arguments | Returns |
|---|---|---|
| `whois_lookup` | `domain` (string) | Registration state (`registered` / `available` / `reserved` / `unsupported` / `unknown`) and the full raw WHOIS/RDAP record: registrar, registration and expiry dates, name servers, status. 1400+ zones including IDN / Cyrillic (`.рф`, `.москва`). RDAP where a zone offers it, plain WHOIS otherwise. |
| `ip_geolocation` | `ip` (string) | Country, region, city, ISP and organisation for an IPv4/IPv6 address. |
| `punycode_convert` | `value` (string) | Converts a domain between Unicode (IDN) and Punycode/ACE (`xn--`), both directions, auto-detected. |

## Manual check

```bash
curl -s https://wwhois.ru/backend/mcp \
  -H 'Content-Type: application/json' -H 'Accept: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Notes

The server is a thin MCP wrapper over wwhois.ru's existing tools — the data is
the same you get from the web pages at
[/whois](https://wwhois.ru/whois), [/ip](https://wwhois.ru/ip) and
[/punycode](https://wwhois.ru/punycode). It is a hosted remote service; this
repository holds the description and manifests only.
