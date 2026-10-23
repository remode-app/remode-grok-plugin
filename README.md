# Remode for Grok

The official [Remode](https://remode.app) plugin for Grok. It connects
Grok to Remode's hosted MCP server, so Grok can find a fashion item from a
photo link or a description, check whether a product is cheaper elsewhere or
second-hand, and keep watching an item for you.

Remode's two stylists present the results: **Colette**, the blunt Parisian
designer who knows what a piece is worth and when to buy it, and **Carrie**,
her warm protégée, who finds the same piece second-hand, for less.

## What it does

- **Find it.** Clothing, shoes, bags, jewelry and accessories from a public
  photo, Instagram, TikTok or Pinterest post, blog or product page, or from a
  plain description ("second-hand Prada loafers, size 39, under 400 CHF").
- **Price it.** Paste one product page and Remode compares the exact item across
  shops and resale marketplaces, with verified exact matches kept apart from
  similar alternatives.
- **Find it second-hand.** New and second-hand listings side by side, so you see
  what the same piece costs on resale.
- **Watch it.** Start a Hunt: Remode keeps checking one exact item for a lower
  price, a restock or a second-hand listing in your size, and reports verified
  changes. Hunts need a Remode account.

Every result comes from a real shop or marketplace listing with its price and
link. Remode does not sell anything; links open the shop. Markets: US, UK,
Germany, Austria and Switzerland, in local currency.

## Contents

| Component | Path | Purpose |
|---|---|---|
| MCP server | `.mcp.json` | Remote server `https://mcp.remode.app/mcp?platform=grok` (streamable HTTP) |
| Skill | `skills/remode/SKILL.md` | When to use Remode, how to call each tool, how to present results |
| Manifest | `.grok-plugin/plugin.json` | Plugin metadata |

### Tools

| Tool | Account | What it does |
|---|---|---|
| `find_fashion_by_description` | no | Find fashion from a text request (brand, model, budget, size, colour, new or second-hand) |
| `find_fashion_items` | no | Find fashion from a photo link, direct image URL, social post, blog or product page |
| `check_price` | no | Compare one product page: same item elsewhere, cheaper or second-hand |
| `watch_item` | yes | Start a Hunt on one exact item (lower price, restock, second-hand) |
| `get_watch_updates` | yes | Read your active Hunts and recorded alerts |
| `update_watch` | yes | Change what an existing Hunt watches for |
| `stop_watching_item` | yes | Stop a Hunt |

## Install

### Grok (grok.com and the Grok apps)

1. Go to [grok.com/connectors](https://grok.com/connectors).
2. Click **New Connector**, then select **Custom**.
3. Name it **Remode** and enter `https://mcp.remode.app/mcp?platform=grok`.

Searching works straight away. Hunt tools ask you to connect your Remode
account (OAuth) the first time you use them. Setup page:
https://remode.app/grok

### Grok Build

Add `https://mcp.remode.app/mcp?platform=grok` as a remote (HTTP) MCP server,
or install this repository as a local plugin to also get the skill. Once Remode
is listed in the Grok Build plugin marketplace:

```bash
grok plugin marketplace add xai-org/plugin-marketplace
grok plugin install remode --trust
```

### Cursor

Add to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "remode": {
      "url": "https://mcp.remode.app/mcp?platform=grok"
    }
  }
}
```

### Any MCP client

Point a streamable-HTTP MCP client at `https://mcp.remode.app/mcp`. Search
works without credentials; Hunt tools return an OAuth challenge with
protected-resource metadata (RFC 9728) when a sign-in is needed.

## Example prompts

- "Where can I buy the jacket in this post? https://www.instagram.com/p/..."
- "Find me a black leather midi skirt under 150 EUR, I'm in Germany."
- "Second-hand Bottega Veneta Jodie under 1,500 CHF."
- "Is this cheaper anywhere? https://www.zara.com/... I'm a size M."
- "Should I buy these boots now or wait? https://www.net-a-porter.com/..."
- "Watch this coat and tell me when it drops under 250 EUR. Size 38."
- "What is Remode watching for me?"
- "Stop the Hunt for the Loewe bag."

## Network endpoints and credentials

- **MCP endpoint:** `https://mcp.remode.app/mcp?platform=grok`, Remode's
  hosted MCP server over HTTPS. Your fashion queries, product links and image
  links go there and nowhere else. `platform=grok` only attributes traffic.
- **Authorization:** only the four Hunt tools need it. They use OAuth 2.1 with
  PKCE (scopes `watch:manage`, `watch:read`), advertised through
  protected-resource metadata on the MCP server and handled by Remode's own
  authorization endpoints. The grant is held by your client and can be revoked
  from your Remode account.
- **Not included:** no hooks, no scripts, no local code execution, no
  filesystem or environment access, no telemetry from the plugin itself.

## Privacy and terms

- Privacy policy: https://remode.app/privacy
- Terms: https://remode.app/terms
- Contact: hello@remode.app

Some shop links may be affiliate links; commercial terms never change the
ranking of results.

## License

MIT. See [LICENSE](LICENSE).
