# Salonnare MCP server

Connect Salonnare to ChatGPT, Claude and any AI agent through Cotlet.dev. Manage bookings, clients, stock and accounting exports in your own signed-in Salonnare account. Write actions ask for approval by default.

## What you can ask

> Fill a cancelled slot from the waitlist.

> Book a regular on a four-week rhythm.

> Reorder products under minimum stock.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Salonnare tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 salonnare_agent.py serve
```

Or with Docker:

```bash
docker build -t salonnare-mcp .
docker run -i --rm -e COTLET_API_KEY salonnare-mcp
```

Tools: `salonnare_guide`, `salonnare_status`, `salonnare_open`, `salonnare_read`, `salonnare_click`, `salonnare_type`, `salonnare_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 salonnare_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 salonnare_agent.py status    # is Salonnare ready in your Cotlet.dev account
python3 salonnare_agent.py guide     # Salonnare screens and sign-in link
python3 salonnare_agent.py read      # open Salonnare, read the page, close the browser
python3 salonnare_agent.py tools     # list Cotlet.dev tools
python3 salonnare_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Salonnare users

**Which Salonnare account does the agent use?**

Your own signed-in Salonnare account in the hosted Cloud Browser.

**Do bookings and refunds need my approval?**

Write actions ask for approval by default, including bookings, receipts, refunds and client emails.

**Is Cotlet affiliated with Salonnare?**

No. Cotlet.dev is independent and not affiliated with Salonnare.

## Learn more

- [Salonnare integration and hosted tools](https://cotlet.dev/integrations/salonnare-api-mcp-server)

---

More: https://cotlet.dev/integrations/salonnare-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=salonnare

Request access: https://cotlet.dev/signup?app=salonnare&utm_source=github&utm_medium=readme&utm_campaign=salonnare

## Workflows on the hosted integration

The live integration page documents these 32 operations:

- `manage_appointments`
- `manage_recurring_series`
- `manage_waitlist`
- `manage_staff`
- `manage_rooms`
- `manage_closed_days`
- `manage_clients`
- `adjust_store_credit`
- `record_colour_formula`
- `manage_loyalty`
- `checkout_sale`
- `void_receipt`
- `refund_receipt`
- `manage_cash_drawer`
- `manage_gift_cards`
- `manage_services`
- `manage_packages`
- `manage_products`
- `record_stock_count`
- `manage_purchase_orders`
- `configure_online_booking`
- `configure_deposits`
- `configure_no_show_fee`
- `create_campaign`
- `manage_email_templates`
- `manage_reviews`
- `manage_promotions`
- `manage_memberships`
- `get_reports`
- `get_insights`
- `read_audit_log`
- `export_accounting_data`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
