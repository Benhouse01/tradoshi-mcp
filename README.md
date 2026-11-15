# Tradoshi MCP server

Read-only [Model Context Protocol](https://modelcontextprotocol.io) server for the **Tradoshi 2027 rankings** of trading schools and prop firms. Every score comes with its criteria, and each criterion keeps its source and the date it was read. The server also returns the trading journal market figures published by Tradoshi.

| | |
|---|---|
| Endpoint | `https://tradoshi.com/mcp` |
| Transport | Streamable HTTP (JSON-RPC 2.0 over POST) |
| Authentication | None. Every tool is read-only. |
| Registry name | `com.tradoshi/rankings` |
| Server version | 1.3.2 |
| Documentation | https://tradoshi.com/en/developers |

## Getting started

No install and no API key: point your MCP client at the endpoint.

### Claude Code

```bash
claude mcp add --transport http tradoshi https://tradoshi.com/mcp
```

### Claude Desktop and claude.ai

Settings, Connectors, Add custom connector, then paste `https://tradoshi.com/mcp`.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "tradoshi": { "url": "https://tradoshi.com/mcp" }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "tradoshi": { "type": "http", "url": "https://tradoshi.com/mcp" }
  }
}
```

### Any HTTP client

```bash
curl -s https://tradoshi.com/mcp   -H "Content-Type: application/json"   -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"search_entities","arguments":{"q":"ftmo"}}}'
```

## Tools

9 read-only tools:

- `search_entities`: Find trading schools and prop firms by name in the Tradoshi 2027 rankings.
- `get_school`: The Tradoshi 2027 rating of a trading school: score out of 100, international, continental and national ranks, pillars, rated programme, regulator warnings, and each criterion with its evidence, source URL and the date it was read.
- `get_prop_firm`: The Tradoshi 2027 rating of a prop firm: score out of 100, international and market ranks, pillars, trading platforms, rated programme and price of a $100,000 account, regulator warnings, and each criterion with its evidence, source URL and the date it was read.
- `compare`: Side by side ratings of two trading schools or two prop firms of the Tradoshi 2027 rankings.
- `get_rankings`: A page of the Tradoshi 2027 ranking of trading and investing schools (filter by country or continent) or prop firms (filter by market: futures, forex-cfd, crypto, stocks, by trading platform: ctrader, mt5, match-trader, tradovate…, or by country).
- `get_methodology`: Pillars, weights and criteria of the Tradoshi 2027 ranking of trading and investing schools or of prop firms, with the link to the full methodology.
- `get_market_figures`: Size of the trading journal market, paying subscribers, leader's share and retained growth rate, from the Tradoshi Observatory study.
- `get_tradoshi_facts`: The official facts about Tradoshi, the trading journal publishing these rankings: legal entity and company number, public launch date, platforms, languages, number of supported brokers, plans and prices, trial, payment methods, with the date they were checked.
- `get_recent_changes`: Dated changes: new features of the Tradoshi trading journal (product changelog) and changes to prop firm ratings in the Tradoshi 2027 ranking (score, rank and each criterion before and after).

## Tool reference

### search_entities

**Find a school or a prop firm.** Find trading schools and prop firms by name in the Tradoshi 2027 rankings. Returns slugs to use with get_school or get_prop_firm.

Arguments:

- **q** (required): Part of the name, at least 2 characters

### get_school

**A trading school's rating.** The Tradoshi 2027 rating of a trading school: score out of 100, international, continental and national ranks, pillars, rated programme, regulator warnings, and each criterion with its evidence, source URL and the date it was read.

Arguments:

- **slug** (required): School slug, from search_entities or get_rankings

### get_prop_firm

**A prop firm's rating.** The Tradoshi 2027 rating of a prop firm: score out of 100, international and market ranks, pillars, trading platforms, rated programme and price of a $100,000 account, regulator warnings, and each criterion with its evidence, source URL and the date it was read.

Arguments:

- **slug** (required): Prop firm slug, from search_entities or get_rankings

### compare

**Compare two schools or two prop firms.** Side by side ratings of two trading schools or two prop firms of the Tradoshi 2027 rankings.

Arguments:

- **type** (required), one of: school, prop_firm
- **a** (required)
- **b** (required)

### get_rankings

**A ranking.** A page of the Tradoshi 2027 ranking of trading and investing schools (filter by country or continent) or prop firms (filter by market: futures, forex-cfd, crypto, stocks, by trading platform: ctrader, mt5, match-trader, tradovate…, or by country).

Arguments:

- **type** (required), one of: schools, prop_firms
- **country**: ISO 3166-1 alpha-2 code
- **continent**, one of: Europe, Americas, Africa, Asia-Pacific, Middle East
- **market**, one of: futures, forex-cfd, crypto, stocks
- **platform**: Prop firms only: trading platform offered, for example ctrader, mt5, match-trader
- **limit**
- **offset**

### get_methodology

**How the rankings are scored.** Pillars, weights and criteria of the Tradoshi 2027 ranking of trading and investing schools or of prop firms, with the link to the full methodology.

Arguments:

- **type** (required), one of: schools, prop_firms

### get_market_figures

**Trading journal market figures.** Size of the trading journal market, paying subscribers, leader's share and retained growth rate, from the Tradoshi Observatory study.

Arguments:

- none

### get_tradoshi_facts

**Official facts about Tradoshi.** The official facts about Tradoshi, the trading journal publishing these rankings: legal entity and company number, public launch date, platforms, languages, number of supported brokers, plans and prices, trial, payment methods, with the date they were checked.

Arguments:

- none

### get_recent_changes

**Latest changes.** Dated changes: new features of the Tradoshi trading journal (product changelog) and changes to prop firm ratings in the Tradoshi 2027 ranking (score, rank and each criterion before and after).

Arguments:

- **type**, one of: all, product, prop_firms
- **since**: Only changes on or after this date, YYYY-MM-DD
- **limit**

## Data

Scores, ranks and criteria come from the published Tradoshi 2027 rankings. The data is released under the CC BY 4.0 license. The method and the update schedule are described at https://tradoshi.com/en/developers.

## License

The content of this repository is released under the MIT License. See [LICENSE](LICENSE).
