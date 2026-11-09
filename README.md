# Zillow Listing Details MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-zillow-listing-details/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-zillow-listing-details) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-zillow-listing-details%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-zillow-listing-details&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-zillow-listing-details)](https://www.npmjs.com/package/@mambalabsdev/mcp-zillow-listing-details) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-zillow-listing-details)](https://www.npmjs.com/package/@mambalabsdev/mcp-zillow-listing-details) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-zillow-listing-details)](https://github.com/mambalabsdev/mcp-zillow-listing-details/blob/main/LICENSE)

An MCP server for the Mamba Labs Zillow Listing Details actor on Apify. Read Zillow home details by URL, ZPID, address, or ZIP code.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give Zillow home URLs, ZPIDs, street addresses, or ZIP codes. You get one flat row per home with price, Zestimate, facts, and status, plus description and the listing agent shown on MLS listings when the home page is read.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-zillow-listing-details": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-zillow-listing-details"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `read_zillow_listings` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What are the price, Zestimate, and listing agent of this Zillow home?"
- "List the homes for sale in ZIP code 78701 with their facts."
- "Which homes recently sold in 60614, and at what price?"

## Tool

`read_zillow_listings`. One tool. Example arguments:

```json
{
  "listings": [
    "97531684"
  ]
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `listings` | array | no | Zillow home URLs, ZPIDs, or street addresses, up to 1,000 per run. An address must include a street number and a city or ZIP code, and it must match the home Zillow returns, or you get a free row. |
| `zip_codes` | array | no | 5 digit US ZIP codes. Each returns the homes on Zillow's search pages for that ZIP code, up to Maximum listings. |
| `listing_status` | string | no | for_sale (default), for_rent, or recently_sold. Used only with ZIP codes. |
| `read_details` | boolean | no | false (default) or true. With false, a ZIP code returns one search row per home at the search row price. With true, the actor reads each home's page at the listing row price, which adds the description and the listing agent. |
| `max_listings` | number | no | The most homes to return in this run, across every input in order, from 1 to 1,000. Default 100. Text or a number. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `listing-row`, one home page read | $0.006 | $0.0057 | $0.0054 | $0.0051 |
| `search-row`, one home from a ZIP code search | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.007 | $0.007 | $0.007 | $0.007 |

Blocked homes, missing homes, unreadable pages, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/zillow-listing-details](https://apify.com/mambalabs/zillow-listing-details)

## License

MIT. See [LICENSE](LICENSE).
