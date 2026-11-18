# ARI MCP server

Connect ARI to ChatGPT, Claude and any AI agent through Cotlet.dev. ARI publishes no public API or MCP server, checked 16 November 2026. The hosted integration documents 14 tools for appointments, inspections, estimates, job cards, invoices, clients, vehicles, parts and reports. Estimates, invoices and anything a client receives wait for your OK.

## What you can ask

> Turn today’s inspection findings into an estimate with authorized, recommended and declined items.

> Start the day from the ARI booking calendar and show cars due for pickup.

> List clients due for service and draft a reminder for my approval.

> Show parts that are running low and prepare a purchase order.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its ARI tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 ari_agent.py serve
```

Or with Docker:

```bash
docker build -t ari-mcp .
docker run -i --rm -e COTLET_API_KEY ari-mcp
```

Tools: `ari_guide`, `ari_status`, `ari_open`, `ari_read`, `ari_click`, `ari_type`, `ari_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 ari_agent.py connect   # ready config for ChatGPT, Claude, Claude Code and Cursor
python3 ari_agent.py status    # is ARI ready in your Cotlet.dev account
python3 ari_agent.py guide     # ARI screens and sign-in link
python3 ari_agent.py read      # open ARI, read the page, close the browser
python3 ari_agent.py tools     # list Cotlet.dev tools
python3 ari_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from ARI users

**Does ARI have a public API or official MCP server?**

ARI publishes no public API or MCP server, checked 16 November 2026. Its API route returned 404 and no developer documentation, API key flow, API pricing or official ChatGPT or Claude connector was verified.

**Where do I sign in to ARI?**

Open https://web.ari.app in your Cotlet Cloud Browser and sign in with your own ARI account.

**Which actions ask for approval?**

Estimates, invoices and every text or email to a client ask for your OK. Write tools ask first by default; you can switch that off per tool.

**Is Cotlet affiliated with ARI?**

No. Cotlet.dev is independent and is not affiliated with ARI.

## Learn more

- [ARI integration and hosted tools](https://cotlet.dev/integrations/ari-api-mcp-server)

---

More: https://cotlet.dev/integrations/ari-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=ari

Request access: https://cotlet.dev/signup?app=ari&utm_source=github&utm_medium=readme&utm_campaign=ari

## Workflows on the hosted integration

The live integration page documents these 14 operations:

- `manage_appointments`
- `manage_inspections`
- `manage_estimates`
- `manage_job_cards`
- `manage_invoices`
- `manage_payments`
- `view_reports`
- `manage_clients`
- `manage_vehicles`
- `manage_service_reminders`
- `manage_inventory`
- `manage_purchase_orders`
- `manage_expenses`
- `restore_deleted_records`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
